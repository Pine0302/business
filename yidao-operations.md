yidaomaster

10016  ssh-copy-id root@yidaomaster
10017  ssh root@yidaomaste
ssh root@node1 ....

3c9c55d407a7        registry.yidaoit.net/ydb/yicli/nginx          "container-entrypoin…"             0.0.0.0:80->8080/tcp, 0.0.0.0:443->8443/tcp   gateway_nginx_1                      nginx 
8804dc0cb101        registry.yidaoit.net/ydb/yicli/php-prod:7.3   "container-entrypoin…"             8080/tcp, 8443/tcp                            yidaoservice_yidaoservice-ydb_1      yidaosaas(生产有客户)
e5f6ad6203f1        plantuml/plantuml-server:tomcat               "catalina.sh run"                  8080/tcp                                      plantuml_plantuml_1					gitlab wiki渲染
ab2939dc7e20        gitlab/gitlab-ce:13.4.3-ce.0                  "/assets/wrapper"                  80/tcp, 443/tcp, 0.0.0.0:9922->22/tcp         gitlab_gitlab-web_1					gitlab
5aa7b5bb5bde        yidaotech/redis                               "container-entrypoin…"             6379/tcp                                      yidaoservice_redis_1					
281f391c820e        yidaotech/memcached                           "docker-entrypoint.s…"             11211/tcp                                     yidaoservice_memcached_1
90ad8f624abd        yidaotech/mysql                               "container-entrypoin…"             3306/tcp                                      yidaoservice_mysql_1
de08762380ce        yidaotech/php-prod                            "container-entrypoin…"             8080/tcp, 8443/tcp                            yidaoservice_yidaoservice-channel_1  已废弃

yidaosaas(生产有客户)  admin kefu888   https://saas.yidaoit.cn/

./yicli-create-repo 要在web新建一个group项目组 新建项目 创建git repo

yicli-init-prod 给客户服务器部署

yicli-init-dev 部署测试环境

创建新项目要新建runner。

node1 : 
1.joolun
2.tianxiawangluo 测试目前停滞
3.cunzhangshuo 停滞
4.xupeitang 停滞
5.kuxuanyizhan 运行
6.yidao-o2o 测试

node2 :
1.kaishanshangcheng 交付中 涛总开发的
2.huixiangshengwu 已交付 汇祥生物 涛总开发的 
3.pinlianshangcheng 交付中 品联 杨总
4.huixiangpintuan 可以停掉
5.paimai 未交付 过嘉峰
6.longjia 二期开发中 过嘉锋
7.youlinxiaodian 使用中 杨总/涛总
8.taolichunfeng 不清楚 
9.tianxiawanglu 停滞

node3：
1.yidaodianshang 一道电商测试环境
2.crmeb 
3.ruoyi
4.weidianxinlingshou 演示可用
5.yidaodianshang 
6.shop_dev 正泰

ssl证书管理：https://code.yidaoit.net/-/snippets/6
/data/docker-projects/gateway/nginx/app-root/etc/letsencrypt/:/opt/app-root/etc/letsencrypt/   --老版本
处理方法

/etc/letsencrypt/:/opt/app-root/etc/letsencrypt/       ---新版本




gitlab 清理磁盘
https://docs.gitlab.com/ee/administration/job_artifacts.html
1.docker exec 进入gitlab
2.gitlab-rails console 打开gitlab的控制台
3.List projects by total size of job artifacts stored    
```
include ActionView::Helpers::NumberHelper
ProjectStatistics.order(build_artifacts_size: :desc).limit(20).each do |s|
  puts "#{number_to_human_size(s.build_artifacts_size)} \t #{s.project.full_path}"
end
```
4.Delete job artifacts from jobs completed before a specific date

```
builds_with_artifacts = Ci::Build.with_downloadable_artifacts
builds_to_clear = builds_with_artifacts.where("finished_at < ?", 1.week.ago)
builds_to_clear.find_each do |build|
  build.artifacts_expire_at = Time.now
  build.erase_erasable_artifacts!
end

```


+ 内存清理
- 清缓存，CI 跑测试总是异常终止，发现 buff/cache 占用份额很大，https://unix.stackexchange.com/questions/87908/how-do-you-empty-the-buffers-and-cache-on-a-linux-system
    ```sh
    free && sync && echo 3 > /proc/sys/vm/drop_caches && free
    ```
+ 杀进程
+ 杀网络连接（禁止ip等）


+ 域名问题

+ 正泰 
1.项目在shop-dev（测试环境）--> merge—request 到 shop（生产）（shop分支代码即将会部署到正泰的3个服务器上面）
2.在本地将feature/deploy rebase shop分支，然后强制推送到gitlab 远程 feature/deploy 


https://cr.console.aliyun.com/repository/cn-hangzhou/yidaoit/annengshop/details   













