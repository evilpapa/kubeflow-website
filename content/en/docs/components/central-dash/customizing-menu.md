+++
title = "自定义菜单"
description = "自定义菜单选项以集成三方应用"
weight = 10
                    
+++

集群管理员可以将第三方应用程序与 kubeflow 集成。在下面的示例中，
“我的应用程序”被添加到侧边菜单栏。

<img src="/docs/images/customize-menu-add-app.png" 
  alt="Display third party app on a kubeflow dashboard"
  class="mt-3 mb-3 border border-info rounded">

## 添加共享项目
本节介绍添加所有用户共享的项目的方法。

首先，集群管理员应该将应用程序部署为 Kubernetes 中的微服务。
应用程序的流量应设置为 Istio 的 VirtualService。

用特定前缀进行部署并通过它控制流量是一种即时方式。
在这种情况下，可以从以下 URL 访问新应用程序。
```
http(s)://gateway/_/myapp/
```

接下来，可以打开菜单栏的配置，如下所示。

```shell
kubectl edit cm centraldashboard-config -n kubeflow
```

您将看到当前设置。请根据需要添加新项目。
```
apiVersion: v1
data:
  links: |-
    {
      "menuLinks": [
        {
          "type": "item",
          "link": "/jupyter/",
          "text": "Notebooks",
          "icon": "book"
        },
.
.
.
        {
          "type": "item",
          "link": "/pipeline/#/executions",
          "text": "Executions",
          "icon": "av:play-arrow"
        },
        {
          "type": "item",
          "link": "/myapp/",
          "text": "MyApp",
          "icon": "social:mood"
        }
      ],
```

"icon" 可以从 iron-icons 中选择。
可在 [icon-demo](http://kevingleason.me/Polymer-Todo/bower_components/iron-icons/demo/index.html) 中查看 Iron-icons 列表。

配置的变化很快就会反映出来。
如果没有，请退出 centraldashboard 并重新加载网络浏览器。

```shell
kubectl rollout restart deployment centraldashboard -n kubeflow
```

您会在菜单栏上看到一个新项目（在本例中为 MyApp）。
通过单击该按钮，您可以 `http(s)://gateway/_/myapp/` 通过 kubeflow 仪表板跳转到并访问第三方应用程序。

## 添加命名空间项
本节介绍了拆分其他应用程序资源的方法。

虽然 Kubeflow 具有多租户功能，但一些第三方应用程序无法与 kubeflow 配置文件交互或不支持多租户。

处理此问题的通用方法是为每个命名空间部署应用程序。
集群管理员为每个命名空间部署应用程序，URL 如下所示。
```
http(s)://gateway/_/myapp/profile1/
http(s)://gateway/_/myapp/profile2/
```

在这种情况下，您可以如下配置看板中心。
当用户打开仪表板时，应将 `{ns}` 替换为命名空间。

```
apiVersion: v1
data:
  links: |-
    {
      "menuLinks": [
        {
          "type": "item",
          "link": "/jupyter/",
          "text": "Notebooks",
          "icon": "book"
        },
.
.
.
        {
          "type": "item",
          "link": "/pipeline/#/executions",
          "text": "Executions",
          "icon": "av:play-arrow"
        },
        {
          "type": "item",
          "link": "/myapp/{ns}",
          "text": "MyApp",
          "icon": "social:mood"
        }
      ],
```

用户可以在菜单栏上看到一个新项目（在这种情况下，它也是 MyApp）。
他们可以基于空间选择跳转到 `http(s)://gateway/_/myapp/profile1/` 或 `http(s)://gateway/_/myapp/profile2/`。
iframe 的实际内部内容由命名空间切换。

如果启用了 sidecar 注入，则对应用的授权由 istio 完成。
比如：不属于 profile2 的用户不能访问 `http(s)://gateway/_/myapp/profile2/`。
