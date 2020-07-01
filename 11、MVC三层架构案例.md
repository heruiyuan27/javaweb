# MVC

<img src="C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200619110729879.png" alt="image-20200619110729879" style="zoom: 67%;" />

# 三层架构

<img src="C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200619110814041.png" alt="image-20200619110814041"  />

# 案例

## 列表查询【升级分页查询】

![image-20200619110929964](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200619110929964.png)

##验证码与登录

验证码

```
<a href="javascript:refreshCode();">
                <img src="${pageContext.request.contextPath}/checkCodeServlet" title="看不清点击刷新" id="vcode"/>
            </a>
############################################################################################################
<script type="text/javascript">
        //切换验证码
        function refreshCode(){
            //1.获取验证码图片对象
            var vcode = document.getElementById("vcode");

            //2.设置其src属性，加时间戳
            vcode.src = "${pageContext.request.contextPath}/checkCodeServlet?time="+new Date().getTime();
        }
    </script>
```

登录

```
LoginServlet：
	Map<String, String[]> map = request.getParameterMap();
    User user = new User();
    BeanUtils.populate(user,map);
    
	UserService service = new UserServiceImpl();
	User loginUser=service.login(user);
############################################################################################################
UserServiceImpl:
private UserDao dao = new UserDaoImpl();
public User login(User user) {
    return dao.findUserByUsernameAndPassword(user.getUsername(),user.getPassword());
}
############################################################################################################
UserDaoImpl:
@Override
    public User findUserByUsernameAndPassword(String username, String password) {
        try {
            String sql = "select * from user where username = ? and password = ?";
            User user = template.queryForObject(sql, new BeanPropertyRowMapper<User>(User.class), username, password);
            return user;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
```

## 增删改

![image-20200622233957225](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200622233957225.png)

![image-20200623001541649](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200623001541649.png)

![image-20200623001619717](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200623001619717.png)

##删除选中

![image-20200626154251348](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200626154251348.png)



##分页查询

<img src="C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200620223618272.png" alt="image-20200620223618272"  />

![image-20200620223738618](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200620223738618.png)

**list.jsp与findUserByPageServlet**

```
    list.jsp提供currentPage与rows
    findUserByPageServlet返回PageBean<User>

public class PageBean<T> {
    private int totalCount;
    private int totalPage;
    private List<T> list;//data each page
    private int currentPage;
    private int rows;//rows each page
}
```

**findUserByPageServlet与UserService**

```
findUserByPageServlet{
	UserService service=new UserServiceImpl();
    PageBean<User> pb=service.findUserByPage(currentPage,rows);
}
	
UserService接口【声明】
	PageBean<User> findUserByPage(String currentPage, String rows);
UserServiceImpl实现类【重写】
	PageBean<User> pb=new PageBean<User>();
    pb.setCurrentPage(currentPage);
    pb.setRows(rows);
    
//totalcount【dao】
    int totalCount=dao.findTotalCount();
    pb.setTotalCount(totalCount);
//list【dao】
    int start = (currentPage-1)*rows;
    List<User> list=dao.findByPage(start,rows);
    pb.setList(list);
//totalpage【计算得到】
    int totalPage= totalCount % rows == 0 ? totalCount/rows :totalCount/rows+1;
    pb.setTotalPage(totalPage);
```

**UserServiceImpl与Dao**

```
UserServiceImpl{
	private UserDao dao = new UserDaoImpl();
	int totalCount=dao.findTotalCount();
	List<User> list=dao.findByPage(start,rows);
}

UserDao接口【声明】
	int findTotalCount();//查询总记录数
    List<User> findByPage(int start, int rows);//分页查询每页记录
UserDaoImpl实现类【重写】
	@Override
    public int findTotalCount() {
        String sql="select count(*) from user";
        return template.queryForObject(sql,Integer.class);
    }

    @Override
    public List<User> findByPage(int start, int rows) {
        String sql ="select * from user limit ? , ? ";
        return template.query(sql,new BeanPropertyRowMapper<User>(User.class),start,rows);
```

## 复杂分页条件查询

![image-20200622203122392](C:\Users\Hery\Desktop\GitHub\javaweb\MVC三层架构案例.assets\image-20200622203122392.png)

findUserByPageServlet

将查询条件也考虑进来

```
	//get query parameters
Map<String, String[]> condition = request.getParameterMap();
	//get PageBean from UserService,forward to jsp
UserService service=new UserServiceImpl();
PageBean<User> pb=service.findUserByPage(currentPage,rows,condition);
request.setAttribute("pb",pb);
request.setAttribute("condition",condition);
request.getRequestDispatcher("/list.jsp").forward(request,response);

```

UserServiceImpl

```
//dao for totalcount
        int totalCount=dao.findTotalCount(condition);
//dao for list
        int start = (currentPage-1)*rows;
        List<User> list=dao.findByPage(start,rows,condition);
```

UserDaoImpl

```
@Override
    public int findTotalCount(Map<String, String[]> condition) {
        //定义模板
        String sql="select count(*) from user where 1 = 1 ";
        StringBuilder sb =new StringBuilder(sql);
        //判断是否有值
        Set<String> keySet = condition.keySet();
        List<Object> params =new ArrayList<Object>();
        for(String key:keySet){
            //排除分页条件参数
            if("currentPage".equals(key) || "rows".equals(key)){
                continue;
            }
            String value= condition.get(key)[0];
            if(value!=null && !"".equals(value)){
                sb.append(" and "+key+" like ? ");
                params.add("%"+value+"%");
            }
        }
        System.out.println(sb.toString());
        System.out.println(params);
        return template.queryForObject(sb.toString(),Integer.class,params.toArray());
    }

```

```
@Override
    public List<User> findByPage(int start, int rows, Map<String, String[]> condition) {
        String sql = "select * from user  where 1 = 1 ";
        StringBuilder sb = new StringBuilder(sql);
        //2.遍历map
        Set<String> keySet = condition.keySet();
        //定义参数的集合
        List<Object> params = new ArrayList<Object>();
        for (String key : keySet) {

            //排除分页条件参数
            if("currentPage".equals(key) || "rows".equals(key)){
                continue;
            }

            //获取value
            String value = condition.get(key)[0];
            //判断value是否有值
            if(value != null && !"".equals(value)){
                //有值
                sb.append(" and "+key+" like ? ");
                params.add("%"+value+"%");//？条件的值
            }
        }
        //添加分页查询
        sb.append(" limit ?,? ");
        //添加分页查询参数值
        params.add(start);
        params.add(rows);
        sql = sb.toString();
        System.out.println(sql);
        System.out.println(params);

        return template.query(sql,new BeanPropertyRowMapper<User>(User.class),params.toArray());
    }
```

