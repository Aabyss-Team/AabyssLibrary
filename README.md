# 使用说明

> 任何问题和新功能添加都可以联系：匿狼(QQ:1927773824)

## 目录介绍

```
AabyssLibrary                               
├─ docs                                     
│  ├─ en（语言：英文）                                    
│  │  └─ home.md（英文默认打开页面）                            
│  ├─ profile（配置文件）                               
│  │  ├─ navbar.md（顶部导航栏配置文件）                          
│  │  └─ sidebar.md（侧边栏配置文件）                         
│  ├─ zh-cn（语言：中文）                                 
│  │  ├─ a-team（渗透测试模块）                             
│  │  ├─ Intranet-sec（内网安全模块）                       
│  │  │  └─ ApacheLog4j2远程代码执行漏洞复现.md（文章）       
│  │  ├─ web-sec（web安全模块）                            
│  │  │  ├─ 三、反序列化+URLDNS预习（java代码审计系列）.md（文章）  
│  │  │  └─ 我的漏洞扫描及安全应急之道.md（文章）                
│  │  └─ home.md（中文默认打开页面）                            
│  └─ index.html（整体页面配置）                            
└─ README.md                                

```

## 上传Markdown文档

1. 从github上clone项目
2. 将自己的markdown文件放到对应的模块目录内(为了更好的分别文章保持项目Markdown文件的摆放整洁)
3. 配置侧边栏配置文件![image-20211226224216492](https://i.niupic.com/images/2021/12/27/9StH.png)
4. push代码git page就会自动更新了



# 环境说明

1. 依赖node.js环境需要自己配好node.js环境

2. 本文库使用文档生成库docsify

   > 使用文档：https://docsify.js.org/#/zh-cn/configuration

3. 

# 本地预览线上环境

在当前项目的根目录运行：`docsify serve docs`

![image-20211226224216492](https://i.niupic.com/images/2021/12/26/9Stx.png)

访问：http://localhost:3000

> 第一次使用请先：`npm i docsify-cli -g`

# 编写注意事项

1. 所有图片需自己上传至图床然后引用图床地址展示图片(图片最好自己做个备份如果图床挂掉的话)
2. Markdown文件可直接**支持中文文件名**不过**文件名不可出现空格**

