

#### eureka 主要对外提供的三个功能

 1. 服务注册
 
 所有的eureka client 向 eureka server 注册信息（ip，port，服务信息）。eureka server会维护一份注册表。
     
 2. 服务发现
 
 eureka client 在调用服务时，会从eureka server获取最新的注册表，并缓存在本地。默认每30秒更新一次。
 
 3. 同步状态
 
 eureka client 默认每30秒发送一次心跳进行服务续约，报告自身的状况。如果client在连续90秒内，没有发送心跳，
 那么server会从注册列表里删除该client。
 
 
