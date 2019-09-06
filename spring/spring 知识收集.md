# spring 知识收集

[TOC]

# spring 与设计模式

[简单工厂模式-spring注入版](https://blog.csdn.net/shiyun123zw/article/details/88413592)

> ```java
> public interface QueryBean {	
> 	public QueryResult query(String json);
> }
> ```
>
> ```java
> @Component("USERLISTQUERYBEAN")
> public class UserListQueryBean implements QueryBean{
> 	@Autowired
> 	private UserService userService;
>  
> 	@Override
> 	public QueryResult query(String json) {
> 		List<User> list = userService.list();
> 		return new QueryResult(true,"",list);
> 	}
> }
> ```
>
> ```java
> @Component
> public class QueryFactory {
> 	@Autowired
> 	private Map<String,QueryBean> map;
> 	
> 	public QueryBean creatQueryBean(String command) {
> 		return map.get(command);
> 	}
> }
> ```
>
> ```java
> @Controller
> @RequestMapping("/zewe")
> public class QueryController {
> 	@Autowired
> 	private QueryFactory queryFactory;
> 	
> 	@RequestMapping(value = "/query", method=RequestMethod.POST)
> 	@ResponseBody
> 	public QueryResult query(@RequestParam("COMMAND") String command,
>                              @RequestParam("CONDITION") String condition) {
> 		QueryBean bean = queryFactory.creatQueryBean(command);
> 		if(null != bean) {
> 			return bean.query(condition);
> 		}
> 		return new QueryResult(false,"内部异常 COMMAND: "+command);	
> 	}
> }
> ```
>
> 



