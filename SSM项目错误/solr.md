# 使用：(配置过程就不说了)

1.进入 cd usr/local/solr/tomcat 启动该solr的tomcat，访问<http://120.79.189.242:8080/solr/>

2.serach-service中添加solrJ的依赖,在spring配置文件中配置初始化*httpSolrServer*类,用于连接solr服务端.

3.服务层方法实例：搜索功能代码

dao：（这里并没有使用mapper类，而是自己写了个dao层方法,使用@Repository注释）

```
@Repository
public class SearchDao {
	
	@Autowired
	private SolrServer solrServer;

	/**
	 *根据查询条件查询索引库
	 * <p>Title: search</p>
	 * <p>Description: </p>
	 * @param query
	 * @return
	 */
	public SearchResult search(SolrQuery query) throws Exception {
		//根据query查询索引库
		QueryResponse queryResponse = solrServer.query(query);
		//取查询结果。
		SolrDocumentList solrDocumentList = queryResponse.getResults();
		//取查询结果总记录数
		long numFound = solrDocumentList.getNumFound();
		SearchResult result = new SearchResult();
		result.setRecordCount(numFound);
		//取商品列表，需要取高亮显示
		Map<String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();
		List<SearchItem> itemList = new ArrayList<>();
		for (SolrDocument solrDocument : solrDocumentList) {
			SearchItem item = new SearchItem();
			item.setId((String) solrDocument.get("id"));
			item.setCategory_name((String) solrDocument.get("item_category_name"));
			item.setImage((String) solrDocument.get("item_image"));
			item.setPrice((long) solrDocument.get("item_price"));
			item.setSell_point((String) solrDocument.get("item_sell_point"));
			//取高亮显示
			List<String> list = highlighting.get(solrDocument.get("id")).get("item_title");
			String title = "";
			if (list != null && list.size() > 0) {
				title = list.get(0);
			} else {
				title = (String) solrDocument.get("item_title");
			}
			item.setTitle(title);
			//添加到商品列表
			itemList.add(item);
		}
		result.setItemList(itemList);
		//返回结果
		return result;
	}
	
}

```

service：

```
Service
public class SearchServiceImpl implements SearchService {
	
	@Autowired
	private SearchDao searchDao;

	@Override
	public SearchResult search(String keyword, int page, int rows) throws Exception {
		//创建一个SolrQuery对象
		SolrQuery query = new SolrQuery();
		//设置查询条件
		query.setQuery(keyword);
		//设置分页条件
		if (page <=0 ) page =1;
		query.setStart((page - 1) * rows);
		query.setRows(rows);
		//设置默认搜索域
		query.set("df", "item_title");
		//开启高亮显示
		query.setHighlight(true);
		query.addHighlightField("item_title");
		query.setHighlightSimplePre("<em style=\"color:red\">");
		query.setHighlightSimplePost("</em>");
		//调用dao执行查询
		SearchResult searchResult = searchDao.search(query);
		//计算总页数
		long recordCount = searchResult.getRecordCount();
		int totalPage = (int) (recordCount / rows);
		if (recordCount % rows > 0) 
			totalPage ++;
		//添加到返回结果
		searchResult.setTotalPages(totalPage);
		//返回结果
		return searchResult;
	}

}

```

