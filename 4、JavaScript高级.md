# DOM简单学习

	* 文档对象模型（Document Object Model，简称DOM）
	
	* 功能：控制html文档的内容
	
	* 获取页面标签(元素)对象：Element
		* document.getElementById("id值"):通过元素的id获取元素对象
	
	* 操作Element对象：
	
		1. 修改属性值：
			1. 明确获取的对象是哪一个？
			2. 查看API文档，找其中有哪些属性可以设置
			
		2. 修改标签体内容：
			* 属性：innerHTML
			1. 获取元素对象
			2. 使用innerHTML属性修改标签体内容
# 事件简单学习

```
* 功能： 某些组件被执行了某些操作后，触发某些代码的执行。
		
	* 如何绑定事件
		1. 直接在html标签上，指定事件的属性(操作)，属性值就是js代码
			<img id="light" src="img/off.gif"  onclick="fun();">
	
		2. 通过js获取元素对象，指定事件属性，设置一个函数
			<img id="light2" src="img/off.gif">
			var light2 = document.getElementById("light2");    
			light2.onclick = fun2;
```

##电灯开关

```html
//1.获取图片对象
var light = document.getElementById("light");
var flag = false;//代表灯是灭的。 off图片

//2.绑定单击事件
light.onclick = function(){
    if(flag){
    	//判断如果灯是开的，则灭掉
        light.src = "img/off.gif";
        flag = false;

    }else{
        //如果灯是灭的，则打开
        light.src = "img/on.gif";
        flag = true;
    }

}
```

#BOM

```
1. 概念：Browser Object Model 浏览器对象模型
	* 将浏览器的各个组成部分封装成对象。

2. 组成：
	* Window：窗口对象
	* Navigator：浏览器对象
	* Screen：显示器屏幕对象
	* History：历史记录对象
	* Location：地址栏对象
```

```
3. Window：窗口对象
    1. 创建
    2. 方法
         1. 与弹出框有关的方法：
            alert()	显示带有一段消息和一个确认按钮的警告框。
            confirm()	显示带有一段消息以及确认按钮和取消按钮的对话框。
                * 如果用户点击确定按钮，则方法返回true
                * 如果用户点击取消按钮，则方法返回false
            prompt()	显示可提示用户输入的对话框。
                * 返回值：获取用户输入的值
         2. 与打开关闭有关的方法：
            close()	关闭浏览器窗口。
                * 谁调用我 ，我关谁
            open()	打开一个新的浏览器窗口
                * 返回新的Window对象
         3. 与定时器有关的方式
            setTimeout()	在指定的毫秒数后调用函数或计算表达式。
                * 参数：
                    1. js代码或者方法对象
                    2. 毫秒值
                * 返回值：唯一标识，用于取消定时器
            clearTimeout()	取消由 setTimeout() 方法设置的 timeout。

            setInterval()	按照指定的周期（以毫秒计）来调用函数或计算表达式。
            clearInterval()	取消由 setInterval() 设置的 timeout。

    3. 属性：
        1. 获取其他BOM对象：
            history
            location
            Navigator
            Screen:
        2. 获取DOM对象
            document
    4. 特点
        * Window对象不需要创建可以直接使用 window使用。 window.方法名();
        * window引用可以省略。  方法名();
```

##轮播图

```html
<body>
    <img id="img" src="img/banner_1.jpg" width="100%">
    <script>
        
        //1.修改图片src属性
        var number = 1;
        function fun(){
            number ++ ;
            //判断number是否大于3
            if(number > 3){
                number = 1;
            }
            //获取img对象
            var img = document.getElementById("img");
            img.src = "img/banner_"+number+".jpg";
        }

        //2.定义定时器
        setInterval(fun,3000);

    </script>
</body>
```

```
4. Location：地址栏对象
	1. 创建(获取)：
		1. window.location
		2. location
	2. 方法：
		* reload()	重新加载当前文档。刷新
	3. 属性
		* href	设置或返回完整的 URL。http://www.baidu.com。
		* port  端口8080
		* protocol  协议http
```

##自动跳转

```html
<body>
    <p>
        <span id="time">5</span>秒之后，自动跳转到首页...
    </p>
    <script>

        var second = 5;
        var time = document.getElementById("time");

        //定义一个方法，获取span标签，修改span标签体内容，时间--
        function showTime(){
            second -- ;
            //判断时间如果<= 0 ，则跳转到首页
            if(second <= 0){
                //跳转到首页
                location.href = "https://www.baidu.com";
            }

            time.innerHTML = second +"";
        }

        //设置定时器，1秒执行一次该方法
        setInterval(showTime,1000);
        
    </script>
</body>
```

```
5. History：历史记录对象
    1. 创建(获取)：
        1. window.history
        2. history

    2. 方法：
        * back()	加载 history 列表中的前一个 URL。
        * forward()	加载 history 列表中的下一个 URL。
        * go(参数)	加载 history 列表中的某个具体页面。
            * 参数：
                * 正数：前进几个历史记录
                * 负数：后退几个历史记录
    3. 属性：
        * length	返回当前窗口历史列表中的 URL 数量。
```

#DOM

```
* 概念： Document Object Model 文档对象模型
	* 将标记语言文档的各个组成部分，封装为对象。可以使用这些对象，对标记语言文档进行CRUD的动态操作

* W3C DOM 标准被分为 3 个不同的部分：

* 核心 DOM - 针对任何结构化文档的标准模型
		* Document：文档对象
		* Element：元素对象
		* Attribute：属性对象
		* Text：文本对象
		* Comment:注释对象

		* Node：节点对象，继承object，是其他5个的父对象。
	* XML DOM - 针对 XML 文档的标准模型
	* HTML DOM - 针对 HTML 文档的标准模型
```

##核心DOM


```
* 核心DOM模型：
	* Document：文档对象
		1. 创建(获取)：在html dom模型中可以使用window对象来获取
			1. window.document
			2. document
		2. 方法：
			1. 获取Element对象：
				1. getElementById()	： 根据id属性值获取元素对象。id属性值一般唯一
				2. getElementsByTagName()：根据元素名称获取元素对象们。返回值是一个数组
				3. getElementsByClassName():根据Class属性值获取元素对象们。返回值是一个数组
				4. getElementsByName(): 根据name属性值获取元素对象们。返回值是一个数组
			2. 创建其他DOM对象：
				createAttribute(name)
            	createComment()
            	createElement()
            	createTextNode()
		3. 属性
	* Element：元素对象
		1. 获取/创建：通过document来获取和创建
		2. 方法：
			1. removeAttribute()：删除属性
			2. setAttribute()：设置属性
	* Node：节点对象，其他5个的父对象
		* 特点：所有dom对象都可以被认为是一个节点
		* 方法：
			* CRUD dom树：
				* appendChild()：向节点的子节点列表的结尾添加新的子节点。
				* removeChild()	：删除（并返回）当前节点的指定子节点。
				* replaceChild()：用新节点替换一个子节点。
		* 属性：
			* parentNode 返回节点的父节点。
```

![image-20200509104432443](C:\Users\Hery\Desktop\GitHub\javaweb\JavaScript高级.assets\image-20200509104432443.png)

## 动态表格

```html
<body>

<div>
    <input type="text" id="id" placeholder="请输入编号">
    <input type="text" id="name"  placeholder="请输入姓名">
    <input type="text" id="gender"  placeholder="请输入性别">
    <input type="button" value="添加" id="btn_add">

</div>


<table>
    <caption>学生信息表</caption>
    <tr>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>操作</th>
    </tr>

    <tr>
        <td>1</td>
        <td>令狐冲</td>
        <td>男</td>
        <td><a href="javascript:void(0);" onclick="delTr(this);">删除</a></td>
    </tr>

    <tr>
        <td>2</td>
        <td>任我行</td>
        <td>男</td>
        <td><a href="javascript:void(0);" onclick="delTr(this);">删除</a></td>
    </tr>

    <tr>
        <td>3</td>
        <td>岳不群</td>
        <td>?</td>
        <td><a href="javascript:void(0);" onclick="delTr(this);" >删除</a></td>
    </tr>


</table>
    
<script>
    //1.获取按钮
    document.getElementById("btn_add").onclick = function(){
        //2.获取文本框的内容
        var id = document.getElementById("id").value;
        var name = document.getElementById("name").value;
        var gender = document.getElementById("gender").value;
        
        //3.创建td，赋值td的标签体
        //id 的 td
        var td_id = document.createElement("td");
        var text_id = document.createTextNode(id);
        td_id.appendChild(text_id);
        //name 的 td
        var td_name = document.createElement("td");
        var text_name = document.createTextNode(name);
        td_name.appendChild(text_name);
        //gender 的 td
        var td_gender = document.createElement("td");
        var text_gender = document.createTextNode(gender);
        td_gender.appendChild(text_gender);
        //a标签的td
        var td_a = document.createElement("td");
        var ele_a = document.createElement("a");
        ele_a.setAttribute("href","javascript:void(0);");
        ele_a.setAttribute("onclick","delTr(this);");
        var text_a = document.createTextNode("删除");
        ele_a.appendChild(text_a);
        td_a.appendChild(ele_a);
        
        //4.创建tr
        var tr = document.createElement("tr");
        
        //5.添加td到tr中
        tr.appendChild(td_id);
        tr.appendChild(td_name);
        tr.appendChild(td_gender);
        tr.appendChild(td_a);
        
        //6.获取table
        var table = document.getElementsByTagName("table")[0];
        table.appendChild(tr);
    }

    //删除方法
    function delTr(obj){
        var table = obj.parentNode.parentNode.parentNode;
        var tr = obj.parentNode.parentNode;

        table.removeChild(tr);
    }

</script>
</body>
```

##HTML DOM

```
* HTML DOM
	1. 标签体的设置和获取：innerHTML
	2. 使用html元素对象的属性
	3. 控制元素样式
		1. 使用元素的style属性来设置
			如：
		        div1.style.border = "1px solid red";
		        div1.style.width = "200px";
		     	div1.style.fontSize = "20px";
		2. 提前定义好类选择器的样式，通过元素的className属性来设置其class属性值。
```

##动态表格2

```html
//使用innerHTML添加
    document.getElementById("btn_add").onclick = function() {
    
        //2.获取文本框的内容
        var id = document.getElementById("id").value;
        var name = document.getElementById("name").value;
        var gender = document.getElementById("gender").value;

        //获取table
        var table = document.getElementsByTagName("table")[0];

        //追加一行
        table.innerHTML += "<tr>\n" +
            "        <td>"+id+"</td>\n" +
            "        <td>"+name+"</td>\n" +
            "        <td>"+gender+"</td>\n" +
            "        <td><a href=\"javascript:void(0);\" onclick=\"delTr(this);\" >删除</a></td>\n" +
            "    </tr>";
    
    }
```

# 事件监听机制

```
* 概念：某些组件被执行了某些操作后，触发某些代码的执行。	
	* 事件：某些操作。如： 单击，双击，键盘按下了，鼠标移动了
	* 事件源：组件。如： 按钮 文本输入框...
	* 监听器：代码。
	* 注册监听：将事件，事件源，监听器结合在一起。 当事件源上发生了某个事件，则触发执行某个监听器代码。
```

```
* 常见的事件：
		1. 点击事件：
			1. onclick：单击事件
			2. ondblclick：双击事件
		2. 焦点事件
			1. onblur：失去焦点
			2. onfocus:元素获得焦点。
	
		3. 加载事件：
			1. onload：一张页面或一幅图像完成加载。
	
		4. 鼠标事件：
			1. onmousedown	鼠标按钮被按下。
			2. onmouseup	鼠标按键被松开。
			3. onmousemove	鼠标被移动。
			4. onmouseover	鼠标移到某元素之上。
			5. onmouseout	鼠标从某元素移开。
		
		5. 键盘事件：
			1. onkeydown	某个键盘按键被按下。	
			2. onkeyup		某个键盘按键被松开。
			3. onkeypress	某个键盘按键被按下并松开。
	
		6. 选择和改变
			1. onchange	域的内容被改变。
			2. onselect	文本被选中。
	
		7. 表单事件：
			1. onsubmit	确认按钮被点击。
			2. onreset	重置按钮被点击。
```

## 表格全选

```html
<script>

    //1.在页面加载完后绑定事件
    window.onload = function(){
       
        document.getElementById("selectAll").onclick = function(){
              //全选
              //1.获取所有的checkbox
              var cbs = document.getElementsByName("cb");
              //2.遍历
                for (var i = 0; i < cbs.length; i++) {
                    //3.设置每一个cb的状态为选中  checked
                    cbs[i].checked = true;
                }
        }

        document.getElementById("unSelectAll").onclick = function(){
            //全不选
            //1.获取所有的checkbox
            var cbs = document.getElementsByName("cb");
            //2.遍历
            for (var i = 0; i < cbs.length; i++) {
                //3.设置每一个cb的状态为未选中  checked
                cbs[i].checked = false;
            }
        }

        document.getElementById("selectRev").onclick = function(){
            //反选
            //1.获取所有的checkbox
            var cbs = document.getElementsByName("cb");
            //2.遍历
            for (var i = 0; i < cbs.length; i++) {
                //3.设置每一个cb的状态为相反
                cbs[i].checked = !cbs[i].checked;
            }
        }

        document.getElementById("firstCb").onclick = function(){
            //第一个cb点击
            //1.获取所有的checkbox
            var cbs = document.getElementsByName("cb");
            //获取第一个cb

            //2.遍历
            for (var i = 0; i < cbs.length; i++) {
                //3.设置每一个cb的状态和第一个cb的状态一样
                cbs[i].checked =  this.checked;
            }
        }

        //给所有tr绑定鼠标移到元素之上和移出元素事件
        var trs = document.getElementsByTagName("tr");
        //2.遍历
        for (var i = 0; i < trs.length; i++) {
            //移到元素之上
            trs[i].onmouseover = function(){
                  this.className = "over";
            }

            //移出元素
            trs[i].onmouseout = function(){
                   this.className = "out";
            }

        }

    }


</script>
```

##表单验证

```html
<script>

    /*
        分析：
            1.给表单绑定onsubmit事件。监听器中判断每一个方法校验的结果。
                如果都为true，则监听器方法返回true
                如果有一个为false，则监听器方法返回false

            2.定义一些方法分别校验各个表单项。
            3.给各个表单项绑定onblur事件。


     */

    window.onload = function(){
        //1.给表单绑定onsubmit事件
        document.getElementById("form").onsubmit = function(){
            //调用用户校验方法   chekUsername();
            //调用密码校验方法   chekPassword();
            return checkUsername() && checkPassword();
        }

        //给用户名和密码框分别绑定离焦事件
        document.getElementById("username").onblur = checkUsername;
        document.getElementById("password").onblur = checkPassword;
    }

    //校验用户名
    function checkUsername(){
        //1.获取用户名的值
        var username = document.getElementById("username").value;
        //2.定义正则表达式
        var reg_username = /^\w{6,12}$/;
        //3.判断值是否符合正则的规则
        var flag = reg_username.test(username);
        //4.提示信息
        var s_username = document.getElementById("s_username");//span

        if(flag){
            //提示绿色对勾
            s_username.innerHTML = "<img width='35' height='25' src='img/gou.png'/>";
        }else{
            //提示红色用户名有误
            s_username.innerHTML = "用户名格式有误";
        }
        return flag;
    }

    //校验密码
    function checkPassword(){
        //1.获取用户名的值
        var password = document.getElementById("password").value;
        //2.定义正则表达式
        var reg_password = /^\w{6,12}$/;
        //3.判断值是否符合正则的规则
        var flag = reg_password.test(password);
        //4.提示信息
        var s_password = document.getElementById("s_password");

        if(flag){
            //提示绿色对勾
            s_password.innerHTML = "<img width='35' height='25' src='img/gou.png'/>";
        }else{
            //提示红色用户名有误
            s_password.innerHTML = "密码格式有误";
        }
        return flag;
    }



</script>
```

