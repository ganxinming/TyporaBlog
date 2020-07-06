### 1.通过ajax进行局部刷新：

```

 <table class="table table-striped table-bordered table-hover" id="sample-table" th:fragment="table_refresh">

$.ajax({
    url: "/deleteAllJtItem",    // 提交到controller的url路径
    type: "post",    // 提交方式
    data: {"array": array},
    dataType:"json",
    traditional:true,//防止深度序列化
    cache:false,
    success:function(data){
        $('#sample-table').load("/Products_List_refresh");
        alert("提交成功");
    },
    async:true,//是否异步
    error : function(xhr) {
        $('#sample-table').load("/Products_List_refresh");
        //window.location.href="Products_List";
        alert("错误提示： " + xhr.status + " " + xhr.statusText);
    }
});

 @RequestMapping("/Products_List_refresh")
    public String Products_List_refresh(Model model) {
        List<JtbItemAndcontent> list=jtbItemService.getAllJtbItemAndContent();
        model.addAttribute("jtbItemList",list);
        return "Products_List::table_refresh";
    }
```

1.使用$('#sample-table').load()加载，其中前面的节点就是要刷新的表格id。

2.表格的头需要加上th:fragment="table_refresh"，表示需要刷新的区域。

3.返回值是"Products_List::table_refresh";  页面::局部区域块