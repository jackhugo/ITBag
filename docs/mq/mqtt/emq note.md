- [扩展插件](http://docs.emqtt.cn/zh_CN/latest/plugins.html)

  EMQ 消息服务器通过模块注册和钩子(Hooks，回调方法)机制，支持用户开发扩展插件定制服务器认证鉴权与业务功能。

  加载、卸载、查询插件:
  ~~~shell
  ./bin/emqttd_ctl plugins load <PluginName>
  ./bin/emqttd_ctl plugins unload <PluginName>
  ./bin/emqttd_ctl plugins list
  ~~~

  用户认证配置两种方法：
  ~~~shell
  etc/plugins/emq_auth_username.conf //修改配置文件
  $ ./bin/emqttd_ctl users add <Username> <Password> //命令行处理
  ~~~

- [测试调优](http://docs.emqtt.cn/zh_CN/latest/tune.html)
