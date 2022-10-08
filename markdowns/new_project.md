---
download: true
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
# class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# colorSchema: 'dark'
# some information about the slides, markdown enabled
info: |
  ## 新项目【创建 - 部署】流程

---

## 新项目【创建 - 部署】流程

---

1. 开发过程
2. 部署过程
3. 验证过程
4. 项目信息配置


---

## <openmoji-face-with-monocle style='display: inline' /> 1.1 创建项目

<v-clicks>

1. 项目组的新项目（不要放在个人账号下），需要在 gitlab 先创建项目，这一步需要找技术负责人。
2. 要确定好项目的分组、命名规范等。
3. 项目创建没权限的话，可以找邓总。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 1.2 Readme

<v-clicks>

1. 在搭建过程中，随时将项目搭建、开发过程中的注意点、规范写入到 Readme 中。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 1.3 接口文档

<v-clicks>

1. 以前是 rap2。
2. 已经明确对接的开发，可以提前开好对应权限，新项目都使用 yapi。
3. yapi 可以自动同步 swagger 的数据。（比如在项目中 grape 接入了 grape-swagger 之类的。在 yapi 里配置同步即可）
4. 尽量保持统一。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.1 域名申请

<v-clicks>

1. 如果需要给外网进行访问的，可以申请域名。（域名申请，需要按规范来命名，写邮件给到邓总，并抄送运维。）
2. 如果只是内部服务（集群）调用，不用申请域名，直接走 service_name 即可。（需要跟运维确定 service_name 的名称）

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.2 提交部署资料（测试、正式）

<v-clicks>

1. 需要先写好 dockerfile (确定好基础镜像，没有基础镜像可以联系运维)，如果一些系统的源比较缓慢，可以进行替换。
2. 在 [2022新项目容器化部署模板](http://confluence.rccchina.com/pages/viewpage.action?pageId=160924261) 填写好相关信息。
3. 在 [资源申请模板](http://confluence.rccchina.com/pages/viewpage.action?pageId=119980412) 填写好相关信息。（PG、Redis等资源）
4. 填完之后，需要下一个 Jira 给到运维，说明信息，并把【2、3】步的文档贴进去。
5. 等待运维进行部署。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.3 部署协助

<v-clicks>

1. 测试环境：new-dev 分支需要提前准备好，运维才能操作。（比如运维要在对应分支添加 .gitlab-ci.yml 之类的）
2. 正式环境：master 分支需要提前准备好。同上。
3. 需要哪些 stages（比如构建、发布，执行迁移等等），是手动自动的方式，这些都是可以提前考虑，同步运维。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.4 配置中心

<v-clicks>

1. 运维会先创建相关项目。然后具体项目需要先上到 new-dev 添加配置。（生产环境的配置也需要同步给运维）

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.5 要部署pod的数量

<v-clicks>

1. 需要提前预估访问量，告知运维设置多少个pod。
2. 每个 pod 需要的 cpu、内存情况。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.6 是否需要监控

<v-clicks>

1. 是否需要监控（grafana大盘等），重要服务还是需要提前加上。
2. [V2集群大盘情况](http://grafana.rccchina.com/d/UVryjpwnz/k8s-monitoring-v2?orgId=1)。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.7 是否需要限流

<v-clicks>

1. 有一定的限流机制。
   - 基于阿里云的ALB
   - [熔断、限流解决方案——traefik](http://confluence.rccchina.com/pages/viewpage.action?pageId=198344707)
   - [dial 平台安全限制](http://confluence.rccchina.com/pages/viewpage.action?pageId=253395198)

2. 可以提前预估服务的访问量，避免一上线出现问题。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.8 是否添加新的请求头

<v-clicks>

1. 如果加了新的请求头，需要运维那边放行。（否则在前端进行调用的时候，会出现跨域的问题）

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.9 健康检查的接口

<v-clicks>

1. 服务需要提供一个 【健康检查】的接口
2. [健康检查接口alive_check](http://confluence.rccchina.com/pages/viewpage.action?pageId=37388897) 、[健康检查的统计](http://confluence.rccchina.com/pages/viewpage.action?pageId=161448990)

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 2.10 oss命名规范

<v-clicks>

1. 如果有使用 oss 进行文件上传，需要命名规范：
   - [OSS的命名规范初稿](http://confluence.rccchina.com/pages/viewpage.action?pageId=158302506)
   - [现有bucket的整理](http://confluence.rccchina.com/pages/viewpage.action?pageId=161448651)

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 3.1 线上验证

<v-clicks>

1. 新服务还未开始使用，可以提前部署。
2. 提前进行验证。（不用等到给到业务系统调用的时候，才发现走不通）
3. 验证的方式：
   - 如果开放了域名，可以使用 postman 进行访问。
   - 如果是 service_name 的方式，可以让运维帮忙在线上服务的容器内，进行 service_name 的访问。（验证容器之间是否能正常访问）
   - 可以准备一个具体请求，让运维在其他服务的控制台（比如rails c）里进行访问。或者直接 curl 进行访问。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 3.2 确认 pod 的数量

<v-clicks>

1. 根据线上情况，需要告知运维 pod 的数量
2. 出现 503 ，很可能是 pod 挂掉了。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 3.3 日志采集验证

<v-clicks>

1. 需要告知运维，要采集哪部分日志。并在线上日志查看。

</v-clicks>

---

## <openmoji-face-with-monocle style='display: inline' /> 4.1 添加新项目 notion 信息

<v-clicks>

1. 像项目管理部发送申请邮件: [新建项目需要录入 notion 的申请邮件](http://confluence.rccchina.com/pages/viewpage.action?pageId=256410777)。

</v-clicks>

