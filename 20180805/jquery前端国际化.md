 什么是国际化？

国际化（Internationalization的缩写是i18n——i,中间18个字符,n）是将软件处理的能让来自各种地方使用各种语言的用户更简单使用的一个过程。

---
  实现方案

  1. 首先将在脚本中用到的文字全部保存到每个语种的文件中,比如简体中文zh_CN.js、英文en.js等等
  2. 然后使用动态语言判断客户端浏览器或Cookies的语言来决定加载对应语种的js文件
  3. 最后把对应js文件中的内容，显示到正确地方

---
 
 如何使用

 1. 引入jquery.i18n.properties-1.0.9.js，jquery-1.11.3.min.js, jquery.json-2.3.min.js
 2. 编写对应的配置文件， message_en.properties , message_zh.properties
 3. jQuery.i18n.browserLang() , 获取对应浏览器语言编码
 4. 根据对应语言编码，加载对应资源文件
     ``` 
        function loadProperties() {
                jQuery.i18n.properties({//加载资浏览器语言对应的资源文件
                    name : 'strings', //资源文件名称
                    path : '/i18n/', //资源文件路径
                    mode : 'map', //用Map的方式使用资源文件中的值
                    language : 'zh',
                    callback : function() {//加载成功后设置显示内容
                    }
                });
            }
    ```
 5. 把要显示的地方，用加载资源文件后内容显示
 
    ```  
        $('#id').text($.i18n.prop('id')); 
    ```
---
  主要方法
  
  1. jQuery.i18n.prop(key)

     - 该方法以map方式使用资源文件中的值，其中key指的是资源文件中的key。当key指定的值含有占位符时，可用使用jQuery.i18n.prop(key,val1,val2……)的形式，其中val1,val2……对点位符进行顺序替换。
   
 2. jQuery.i18n.browserLang()
  
    - 用于获取浏览器的语言信息。
---    
 FAQ

 1. 区分简体中文与繁体中文
   - 简体中文对应的资源文件 messages_zh-CN.properties ; 繁体中文对应的资源文件 messages_zh_TW.properties 

 2. 国际化同时，传入实际数据显示
   - string_hello= 您好 {0}，歡迎使用 jQuery.i18n.properties，您的密鑰是：{1}。

   - $('#id').text($.i18n.prop('string_hello','名称'，‘密码’)); 
    
   