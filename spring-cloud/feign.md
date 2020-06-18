

#### 什么是feign？

一个轻量级的http请求调用框架。其实质是封装了http网络请求框架（HttpURLConnection或Okhttp或HttpClient），可以以注解的方式去声明http
请求。

按照Feign规则实现接口方法，接口方法被调用时，Feign 通过处理注解生成Request模板，在发送请求之前，再通过处理注解替换Request模板中的参数，生成真正的
Request对象，并交给对应的Http请求框架Client（HttpUrlConnection或Okhttp或HttpClient）处理。

#### feign 实现负载均衡

真正的Request对象会交给Client去处理，而Client最后被封装到了LoadBalencerClient类，这个类结合
Ribbo 做到了负载均衡。


