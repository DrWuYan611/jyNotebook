### 一个简单的盒子

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
div{
    width:100px;
    height: 100px;
    border: 1px solid black;
}

</style>
</head>
<body>
<div></div>
</body>

</html>
```

![](/assets/14.2.10.3-01.png)

---

### 单位

* 绝对单位
  * cm
  * ..
* 相对单位
  * em
  * ..

---

#### 文本缩进

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
div{
    line-height: 16px;
    height: 200px;
    border: 1px solid black;
    text-align: left;/*左对齐*/
    text-indent: 2em;/*文本缩进*/
}

</style>
</head>
<body>
<div>道者，令民與上同意，可與之死，可與之生，而不畏危也。天者，陰陽、寒暑、時制也。地者，高下、遠近、險易、廣狹、死生也。將者，智、信、仁、勇、嚴也。法者，曲制、官道、主用也。凡此五者，將莫不聞，知之者勝，不知者不勝。故校之以計而索其情，曰：主孰有道？將孰有能？天地孰得？法令孰行？兵眾孰強？士卒孰練？賞罰孰明？吾以此知勝負矣</div>
</body>

</html>
```

![](/assets/14.2.10.3-02.png)

### text-decoration的简单用法

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">

    span{
        text-decoration: line-through;
    }

    del{
        text-decoration: none;
    }

</style>
</head>
<body>
    <del>原价50元</del>
    <span>原价50元</span>
</body>
</html>
```

![](/assets/14.2.10.3-03.png)

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
    span{
        color: rgb(0,0,238);
        text-decoration: underline;
        cursor: help;/*光标*/
    }
</style>
</head>
<body>
    <span>www.baidu.com</span>
    <a href="www.baidu.com">www.baidu.com</a>
</body>
</html>
```

![](/assets/14.2.10.3-04.png)

### 伪类选择器

* :hover

```
当鼠标移动此处时，改变样式

a:hover{
    background:red;
}
```

### 用法

* 行级元素:span、strong、em、a、del
  * feature:
    * 内容决定元素所占位置
    * 不可以通过css改变宽高
* 块级元素:div、p、ul、li、form、ol、address

  * feature
    * 独占一行
    * 可以通过css改变宽高

* 行级块元素:inline-block

  * feature:
    * 内容决定大小
    * 可以改变宽高

```
1.通过display:block修改为块级元素

<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
span{
display: block;
width: 100px;
height: 100px;
background: red;
}
</style>
</head>
<body>
<span>CoderAngle</span>
</body>
</html>



2.通过display:inline修改为行级元素


<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
span{
display: inline;
width: 100px;
height: 100px;
background: red;
}
</style>
</head>
<body>
<span>CoderAngle</span>
</body>
</html>
```

---

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
	.box1{
		width: 100px;
		height: 100px;
		background: red;
	}
	
	.box2{
		width: 200px;
		height: 200px;
		background: red;
	}
	
	.box3{
		width: 300px;
		height: 300px;
		background: red;
	}
	
	.box4{
		width: 100px;
		height: 100px;
		background: red;
	}
	
	.box5{
		width: 200px;
		height: 200px;
		background: red;
	}
</style>
</head>
<body>
	<div class="box1"></div>
	<div class="box2"></div>
	<div class="box3"></div>
	<div class="box4"></div>
	<div class="box5"></div>
</body>
</html>
```

样式表看起来有点冗余

* 先定义功能后，后组合
  * 方便，便于维护

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
	.green{
		background-color: green;
	}
	
	.gray{
		background-color: gray;
	}
	
	.red{
		background-color: red;
	}
	
	.size1{
		width: 100px;
		height: 100px;
	}
	
	.size2{
		width: 200px;
		height: 200px;
	}
	
	.size3{
		width: 300px;
		height: 300px;
	}
</style>
</head>
<body>
	<div class="red size1"></div>
	<div class="gray size2"></div>
	<div class="green size3"></div>
	<div class="red size1"></div>
	<div class="green size2"></div>
</body>
</html>
```

### 自定义功能

* 初始化标签

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
	em{
		font-style: normal;
		color: #FF0000;
	}
	
	a{
		text-decoration: none;
		color: #c00;
	}
	
	ul{
		list-style: none;
		padding: 0;
		margin: 0;
	}
</style>
</head>
<body>
	<em>angle</em>
	<a href="">www.baidu.com</a>
	<ul>
		<li></li>
		<li></li>
		<li></li>
	</ul>
</body>
</html>
```

* 定义所有标签初始化默认样式

```
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<title></title>
<style type="text/css">
	
	*{
		padding: 0;
		margin: 0;
		text-decoration: none;
		list-style: none;
	}
</style>
</head>
<body>
	<em>angle</em>
	<a href="https://www.baidu.com">www.baidu.com</a>
	<ul>
		<li></li>
		<li></li>
		<li></li>
	</ul>
</body>
</html>
```


