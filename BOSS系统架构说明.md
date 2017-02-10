---
title: BOSS系统架构说明
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

# boss系统模块
![enter description here][1]

如上图所示，目前BOSS系统主要包括：客户报名、预约管理、上门管理、报表统计和系统设置模块，其余模块已经不使用。

系统流程可理解为四个主要流程：
* 客户报名流程
* 用户（员工）对报名客户操作流程
* 运营人员了解和查看相关客户跟进信息统计流程
* UC权限控制流程

针对以上四点，对BOSS系统进行如下说明

## 客户报名
![enter description here][2]
客户通过我们线上相关网站填写电话、城市、面积等信息以后，前台主站对信息处理和分渠道以后，提交json形式信息到`SourceController.class.php`控制器`insert`方法，系统录入数据库后执行去重、判断无效、自动分配等操作；保存客户信息到BOSS系统。

## boss系统目前代码结构
我们公司员工，在访问BOSS系统执行相关工作时，就开始了BOSS系统的主要逻辑。一下从员工登录boss系统开始进行相关说明：

* 登录和信息同步
![enter description here][3]
员工在登录boss系统时，会去UC系统验证身份；验证通过后会同步登录账户相关信息到BOSS系统，如果登录用户包含下级用户，会同时将下级用户的信息同步到BOSS系统；   
用户在登录从UC验证成功以后，UC系统会返回给BOSS系统一个有过期时间的token，boss保持登录用户的token一遍后续向UC查询权限使用。登录成功以后，系统同时会请求UC获取左侧菜单栏有哪些，保存session中，进入系统，根据session保存设定左侧可显示的菜单栏。

* 系统逻辑代码结构
![enter description here][4]
BOSS系统使用thinkphp框架，thinkphp是MVC框架，即：视图-模型-逻辑。上图列举出了boss系统目前主要模块的MVC划分图，说明一下：
用户在登录系统`LoginController.class.php`成功后，进入到一个由`BaseController.class.php`基类控制的相关控制器中；
业务逻辑代码控制器继承于基类`BaseController.class.php`，调用数据模型层**Model**操作数据库，调用**view**向客户端输出渲染后页面。  
视图层**view** 采用thinkphp本身模板引擎进行渲染，采用静态模板缓存（Runtime）加速网站性能


  [1]: ./images/BOSS%E7%B3%BB%E7%BB%9F.png "BOSS系统.png"
  [2]: ./images/BOSS%E7%B3%BB%E7%BB%9F%E5%AE%A2%E6%88%B7%E6%8A%A5%E5%90%8D%E6%95%B0%E6%8D%AE%E6%B5%81.png "BOSS系统客户报名数据流.png"
  [3]: ./images/boss-uc%E6%9D%83%E9%99%90%E4%BA%A4%E4%BA%92%E6%B5%81%E7%A8%8B.png "boss-uc权限交互流程.png"
  [4]: ./images/BOSS%E7%B3%BB%E7%BB%9F%E4%BB%A3%E7%A0%81%E7%BB%84%E7%BB%87%E7%BB%93%E6%9E%84.png "BOSS系统代码组织结构.png"