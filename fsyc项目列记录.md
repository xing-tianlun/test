## fsyc项目列记录

1. official-website-show2

企业官网   <span style="color: red"> check</span>

2. smart-vegetable-garden

智慧菜园 jQ  <span style="color: red"> check</span>

3. **施工运维一体化web端**

construction-operation-integration-admin   <span style="color: red"> check</span>

4. **数字库房管理系统（专业仓项目）**

dwms <span style="color: red"> check</span>

5. **施工运维一体化小程序端**

小程序管理员账号登录不上

git: http://gitlab-bj.forsaven.com/iot/web/mobilewarehouse.git

6. **数据采集大屏展示项目**

无源码

项目访问路径：http://192.168.8.195:85/collect-data-screen/#/index

7. **新田数据可视化大屏**

已运行  没有数据返回  请求地址有问题

http://gitlab-bj.forsaven.com/iot/edge-forsaven-ui.git

这个地址 master 分支 和 devGYY 分支不是一个项目



8. **旧边缘计算平台**

edge-forsaven-ui    <span style='color: red'>check</span>

```js
// 修改地址后可以登录
config/index.js

:14
proxyTable: {
    "/api": {
    	// target: 'http://123.57.153.237:22114/',*
	   target: 'http://47.110.12.205:22038/',

http://47.110.12.205:22038/  //地址


master: 主分支暂时没有用到，用开发分支部署上线的，项目初始化后一直没有合并过分支，分支上有修改过
'devGYY': 最新的分支，用于最新版本的开发分支
'devJXD': 旧分支，修改过一些，但未合并
'dev': 旧分支
```



http://gitlab-bj.forsaven.com/iot/edge-forsaven-ui.git

http://47.110.12.205:22038/dist/#/

admin/123456



devGYY分支

![image-20240227105955547](/Users/xingtianlun/Library/Application Support/typora-user-images/image-20240227105955547.png)

通过更改地址，不需要登录就可进入页面。



9. **碳资产云管理平台**

carbon-assert-web   <span style='color: red'>check</span>

git 地址: http://gitlab-bj.forsaven.com/xian/carbon-assert-web.git

项目访问地址:  http://47.110.12.205:22021/carbon_assert_web/index.html

```js
master 主分支：暂时没有用到，用开发分支部署上线的
'dev': 旧分支
'devGYY': 最新的分支，用于最新版本的开发分支
'mengxi-test'：比较旧的分支，修改过一些问题但未合并分支
'yxl': 比较旧的分支，增加了一些校验，已合并分支。
```



10. **采集管理平台**

    iot-upgrade  不需要

git 地址: http://gitlab-bj.forsaven.com/xian/all-link-web.git  里面的 electric 文件是：采集管理平台

项目访问地址:

http://47.110.12.205:22021/iot_master_web/index.html

admin/fs88888888



collect 文件是：能源管理系统

11. **国网综能**

sgcc-web

```js
master: 主分支，旧分支，暂时没有用到，用开发分支部署上线的
'devGYY': 最新的分支，用于最新版本的开发分支
'develop': 旧分支
'feature-jxd': 旧分支，已合并
'feaure-alex': 旧分支，已合并
'test': 旧分支
'release': 旧分支
```

账号SuperAdmin/88888888

部署在8.182服务器上，centos-7操作系统

项目访问地址:  http://47.110.12.205:22021/sgcc_web/index.html

git 地址: http://gitlab-bj.forsaven.com/xian/sgcc-web.git



12. **电力能耗数据采集系统**

antai-web

git 地址: http://gitlab-bj.forsaven.com/xian/antai-web.git

项目访问地址: http://47.110.12.205:22021/antai_view_web/ 项目预览地址不对，不是同一个项目

admin/88888888

