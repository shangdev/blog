---
title: jquery.form.js使用心得
date: 2017-03-13 22:33:27
tags:
- 前端
categories:
- 前端相关
---

官网地址：[jquery_form官网](http://jquery.malsup.com/form/)

参数说明：
```
    target        返回的结果将放到这个target下
    url           如果定义了，将覆盖原form的action
    type          get和post两种方式
    dataType      返回的数据类型，可选：json、xml、script
    clearForm     true，表示成功提交后清除所有表单字段值
    resetForm     true，表示成功提交后重置所有字段
    iframe        如果设置，表示将使用iframe方式提交表单
    beforeSerialize    数据序列化前：function($form,options){}
    beforeSubmit  提交前：function(arr,$from,options){}
    success       提交成功后：function(data,statusText){}
    error         错误：function(data){alert(data.message);}  
```
使用方法：

<!-- more -->

```
var options = { 
    url:'{:url('Company/add')}',  
    type:'post',
    dataType:'json',
    beforeSubmit:function(arr,$from,options){
        for(var i=0;i<arr.length;i++){}
    },
    success: function(data,statusText){
        if(data.state==1){}
    },
    error: function(data){console.log(data.message);}
}; 
$('form').submit(function() { 
    $(this).ajaxSubmit(options); 
    return false; 
});

```