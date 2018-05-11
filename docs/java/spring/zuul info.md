- [zuul quick start](https://spring.io/guides/gs/routing-and-filtering/)
- [spring-cloud-zuul-ratelimit 限流](https://github.com/marcosbarbero/spring-cloud-zuul-ratelimit)
- [Uploading Files through Zuul](http://cloud.spring.io/spring-cloud-static/spring-cloud.html#_uploading_files_through_zuul)
- [Zuul超时问题，微服务响应超时](https://blog.csdn.net/tianyaleixiaowu/article/details/78772269)

ribbon.ReadTimeout， ribbon.SocketTimeout这两个就是ribbon超时时间设置，当在yml写时，应该是没有提示的，给人的感觉好像是不是这么配的一样，其实不用管它，直接配上就生效了。
还有zuul.host.connect-timeout-millis， zuul.host.socket-timeout-millis这两个配置，这两个和上面的ribbon都是配超时的。区别在于，如果路由方式是serviceId的方式，那么ribbon的生效，如果是url的方式，则zuul.host开头的生效。（此处重要！使用serviceId路由和url路由是不一样的超时策略）
如果你在zuul配置了熔断fallback的话，熔断超时也要配置，不然如果你配置的ribbon超时时间大于熔断的超时，那么会先走熔断，相当于你配的ribbon超时就不生效了。

熔断超时是这样的：
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds: 60000
