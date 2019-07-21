# 使用：

1.添加依赖

2.创建一个.ftl文件。如下：

```
${hello}
```

3.测试：

```
@Test
public void testFreeMarker() throws Exception {
   //1、创建一个模板文件
   //2、创建一个Configuration对象
   Configuration configuration = new Configuration(Configuration.getVersion());
   //3、设置模板文件保存的目录
   configuration.setDirectoryForTemplateLoading(new File("B:\\gan\\e3\\item-web\\src\\main\\webapp\\WEB-INF\\ftl"));
   //4、模板文件的编码格式，一般就是utf-8
   configuration.setDefaultEncoding("utf-8");
   //5、加载一个模板文件，创建一个模板对象。
   Template template = configuration.getTemplate("hello.ftl");
   //6、创建一个数据集。可以是pojo也可以是map。推荐使用map
   Map data = new HashMap<>();
   data.put("hello", "hello freemarker!");
   //7、创建一个Writer对象，指定输出文件的路径及文件名。
   Writer out = new FileWriter(new File("B:\\gan\\e3\\item-web\\src\\main\\webapp\\WEB-INF\\jsp\\hello.html"));
   //8、生成静态页面
   template.process(data, out);
   //9、关闭流
   out.close();
}
```

#### 与spring整合：applicationContext-freemarker.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
   xmlns:mvc="http://www.springframework.org/schema/mvc"
   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
      http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd">

   <!-- 配置freemarker -->
   <bean id="freemarkerConfig"
        class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
      <property name="templateLoaderPath" value="/WEB-INF/ftl/" />
      <property name="defaultEncoding" value="UTF-8" />
   </bean></beans>
```

#### 使用：

```
/**
 * 生成静态页面测试Controller
 * <p>Title: HtmlGenController</p>
 * <p>Description: </p>
 * <p>Company: www.itcast.cn</p> 
 * @version 1.0
 */
@Controller
public class HtmlGenController {

	@Autowired
	private FreeMarkerConfigurer freeMarkerConfigurer;
	
	@RequestMapping("/genhtml")
	@ResponseBody
	public String genHtml() throws Exception {
		Configuration configuration = freeMarkerConfigurer.getConfiguration();
		//加载模板对象
		Template template = configuration.getTemplate("hello.ftl");
		//创建一个数据集
		Map data = new HashMap<>();
		data.put("hello", 123456);
		//指定文件输出的路径及文件名
		Writer out = new FileWriter(new File("D:/temp/JavaEE32/freemarker/hell2.html"));
		//输出文件
		template.process(data, out);
		//关闭流
		out.close();
		
		return "OK";
	}
}

```

### 思考：

在spring配置freemaker后，直接通过freeMarkerConfigurer获得Configuration，然后加载模板文件。

模板文件.ftl：需要我们事先写好。然后将数据和输出文件绑定，这样就生成了所需的页面。