Cascading Style Sheets 层叠样式表

 层叠：多个样式可以作用在同一个html的元素上同时生效

# CSS&html结合方式

##1. 内联样式
* 在标签内使用style属性指定css代码 
```html
<div style="color:red;">hello css</div>
```
##2. 内部样式
* 在head标签内，定义style标签，style标签的标签体内容就是css代码
```html
<style>
div{
	color:blue;
}	
</style>

<div>hello css</div>
```
##3. 外部样式
- 定义css资源文件。
```html
a.css文件：
div{
	color:green;
}
```
- 在head标签内，定义link标签，引入外部的资源文件
```html
<link rel="stylesheet" href="css/a.css">
<div>hello css</div>
<div>hello css</div>
```

* 1,2,3种方式 css作用范围越来越大
* 1方式不常用，后期常用2,3
* 3种格式可以写为：
```
<style>
	@import "css/a.css";
</style>
```

#CSS语法

##1. 基础选择器

id选择器，元素选择器，类选择器，其中元素选择器优先级最低

```
<head>
    <meta charset="UTF-8">
    <title>基础选择器</title>

    <style>

        .cls1{

            color: blue;
        }

        div{
            color:green;
        }

        #div1{
            color: red;
        }

    </style>
    
</head>

<body>

    <div id="div1">传智播客</div>
    <div class="cls1">黑马程序员</div>
    <p class="cls1">传智学院</p>

</body>
```

## 2.  扩展选择器

子选择器，父选择器，属性选择器，伪类选择器

```
<head>
    <meta charset="UTF-8">
    <title>扩展选择器</title>

    <style>
        div p{
            color:red;
        }

        div > p {
            border: 1px solid;
        }

        input[type='text']{
            border: 5px solid;
        }

        a:link{
            color: pink;
        }

        a:hover{
            color: green;
        }

        a:active{
            color: yellow;
        }

        a:visited{
            color: red;
        }

    </style>
</head>

<body>

    <div>
        <p>传智播客</p>
    </div>
    <p>黑马程序员</p>
	<div>helloworld</div>

    <input type="text" >
    <input type="password" >

    <br>

    <a href="#">黑马程序员</a>

</body>
```

## 样式属性

```
<style>
    p{
        color: #FF0000;
        font-size: 30px;
        text-align: center;
        line-height: 200px;
        border: 1px solid red;
    }
    div{
        border: 1px solid red;
        height: 200px;
        width: 200px;
        background: url("img/logo.jpg") no-repeat center;
    }
</style>
```

```
<style>
    div{
        border: 1px solid red;
        width: 100px;
    }
    .div1{
        width: 100px;
        height: 100px;
        /*外边距*/
        <!--margin: 50px;-->
    }
    .div2{
        width: 200px;
        height: 200px;
        padding: 50px;
        /*
            设置盒子的属性，让width和height就是最终盒子的大小
         */
        box-sizing: border-box;
    }

    .div3{
        float: left;
    }

    .div4{
        float: left;
    }

    .div5{
        float: right;
    }

</style>
```

# 注册页面

```html
<!DOCTYPE html>
<html lang="ch">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
    <style>
    *{
        margin: 0px;
        padding: 0px;
        box-sizing: border-box;
    }
    body{
        background: url("img/sea.jpg") no-repeat center;
        padding-top: 25px;
    }
    .rg_layout{
        width: 900px;
        height: 500px;
        border: 8px solid #EEEEEE;
        background-color: white;
        /*让div水平居中*/
        margin: auto;
    }

    .rg_left{
        /*border: 1px solid red;*/
        float: left;
        margin: 15px;
    }
    .rg_left > p:first-child{
        color:#FFD026;
        font-size: 20px;
    }

    .rg_left > p:last-child{
        color:#A6A6A6;
        font-size: 20px;
    }

    .rg_center{
        float: left;
       /* border: 1px solid red;*/

    }

    .rg_right{
        /*border: 1px solid red;*/
        float: right;
        margin: 15px;
    }

    .rg_right > p:first-child{
        font-size: 15px;

    }
    .rg_right p a {
        color:pink;
    }

    .td_left{
        width: 100px;
        text-align: right;
        height: 45px;
    }
    .td_right{
        padding-left: 50px ;
    }
    
    #username,#password,#email,#name,#tel,#birthday,#checkcode{
        width: 251px;
        height: 32px;
        border: 1px solid #A6A6A6 ;
        /*设置边框圆角*/
        border-radius: 5px;
        padding-left: 10px;
    }
    #checkcode{
        width: 110px;
    }

    #img_check{
        height: 32px;
        vertical-align: middle;
    }
    #btn_sub{
        width: 150px;
        height: 40px;
        background-color: #FFD026;
        border: 1px solid #FFD026;
    }


    </style>
</head>
<body>
<div class="rg_layout">

    <div class="rg_left">
        <p>新用户注册</p>
        <p>USER REGISTER</p>
    </div>

    <div class="rg_center">
        <div class="rg_form">
            <form action="#" method="post">
                <table>
                    <tr>
                        <td class="td_left"><label for="username">用户名</label></td>
                        <td class="td_right"><input type="text" name="username" id="username" placeholder="请输入用户名"></td>
                    </tr>

                    <tr>
                        <td class="td_left"><label for="password">密码</label></td>
                        <td class="td_right"><input type="password" name="password" id="password" placeholder="请输入密码">
                        </td>
                    </tr>

                    <tr>
                        <td class="td_left"><label for="email">Email</label></td>
                        <td class="td_right"><input type="email" name="email" id="email" placeholder="请输入邮箱"></td>
                    </tr>

                    <tr>
                        <td class="td_left"><label for="name">姓名</label></td>
                        <td class="td_right"><input type="text" name="name" id="name" placeholder="请输入姓名"></td>
                    </tr>

                    <tr>
                        <td class="td_left"><label for="tel">手机号</label></td>
                        <td class="td_right"><input type="text" name="tel" id="tel" placeholder="请输入手机号"></td>
                    </tr>

                    <tr>
                        <td class="td_left"><label>性别</label></td>
                        <td class="td_right">
                            <input type="radio" name="gender" value="male"> 男
                            <input type="radio" name="gender" value="female"> 女
                        </td>
                    </tr>

                    <tr>
                        <td class="td_left"><label for="birthday">出生日期</label></td>
                        <td class="td_right"><input type="date" name="birthday" id="birthday" placeholder="请输入出生日期">
                        </td>
                    </tr>

                    <tr>
                        <td class="td_left"><label for="checkcode">验证码</label></td>
                        <td class="td_right"><input type="text" name="checkcode" id="checkcode" placeholder="请输入验证码">
                            <img id="img_check" src="img/verify_code.jpg">
                        </td>
                    </tr>


                    <tr>
                        <td colspan="2" align="center"><input type="submit" id="btn_sub" value="注册"></td>
                    </tr>
                </table>
            </form>
        </div>
    </div>

    <div class="rg_right">
        <p>已有账号?<a href="#">立即登录</a></p>
    </div>

</div>


</body>
</html>
```