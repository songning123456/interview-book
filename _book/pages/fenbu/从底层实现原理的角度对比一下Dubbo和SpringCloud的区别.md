#### Dubbo && SpringCloud原理图
![Dubbo && SpringCloud原理图](/images/分布式/Dubbo&SpringCloud.png)
    
#### RPC && SpringCloud原理图
![RPC && SpringCloud原理图](/images/分布式/RPC&SpringCloud.PNG)
    
#### 相同点
&emsp;&emsp;都需要 服务提供方，注册中心，服务消费方。
    
#### Dubbo
&emsp;&emsp;(1) Provider: 暴露服务的提供方，可以通过jar或者容器的方式启动服务。<br/>
&emsp;&emsp;(2) Consumer: 调用远程服务的服务消费方。<br/>
&emsp;&emsp;(3) Registry: 服务注册中心和发现中心。<br/>
&emsp;&emsp;(4) Monitor: 统计服务和调用次数，调用时间监控中心。(dubbo的控制台页面中可以显示，目前只有一个简单版本)<br/>
&emsp;&emsp;(5) Container: 服务运行的容器。<br/>
    
#### Spring Cloud
&emsp;&emsp;(1) Service Provider: 暴露服务的提供方。<br/>
&emsp;&emsp;(2) Service Consumer: 调用远程服务的服务消费方。<br/>
&emsp;&emsp;(3) EureKa Server: 服务注册中心和服务发现中心。<br/>
    
#### 比较
&emsp;&emsp;(1) 从核心要素来看<br/>
&emsp;&emsp;Spring Cloud 更胜一筹，在开发过程中只要整合Spring Cloud的子项目就可以顺利的完成各种组件的融合，而Dubbo缺需要通过实现各种Filter
来做定制，开发成本以及技术难度略高。Dubbo只是实现了服务治理，而Spring Cloud子项目分别覆盖了微服务架构下的众多部件，而服务治理只是其中
的一个方面。Dubbo提供了各种Filter，对于上述中“无”的要素，可以通过扩展Filter来完善。<br/>
&emsp;&emsp;e.g:<br/>
&emsp;&emsp;&emsp;&emsp;(a) 分布式配置：可以使用淘宝的diamond、百度的disconf来实现分布式配置管理<br/>
&emsp;&emsp;&emsp;&emsp;(b) 服务跟踪：可以使用京东开源的Hydra，或者扩展Filter用Zippin来做服务跟踪<br/>
&emsp;&emsp;&emsp;&emsp;(c) 批量任务：可以使用当当开源的Elastic-Job、tbschedule<br/>
&emsp;&emsp;(2) 从协议上看<br/>
&emsp;&emsp;Dubbo缺省协议采用单一长连接和NIO异步通讯，适合于小数据量大并发的服务调用，以及服务消费者机器数远大于服务提供者机器数的情况，二进
制的传输，占用带宽会更少。Spring Cloud 使用HTTP协议的REST APIdubbo支持各种通信协议，而且消费方和服务方使用长链接方式交互，http协议
传输，消耗带宽会比较多，同时使用http协议一般会使用JSON报文，消耗会更大。通信速度上Dubbo略胜Spring Cloud，如果对于系统的响应时间有严
格要求，长链接更合适。<br/>
&emsp;&emsp;(3) 从服务依赖方式看<br/>
&emsp;&emsp;Dubbo服务依赖略重，需要有完善的版本管理机制，但是程序入侵少。而Spring Cloud通过Json交互，省略了版本管理的问题，但是具体字段含义需
要统一管理，自身Rest API方式交互，为跨平台调用奠定了基础。<br/>
&emsp;&emsp;(4) 从组件运行流程看<br/>
&emsp;&emsp;Dubbo每个组件都是需要部署在单独的服务器上，gateway用来接受前端请求、聚合服务，并批量调用后台原子服务。每个service层和单独的DB交互。<br/>
    
&emsp;&emsp;SpringCloud所有请求都统一通过 API 网关(Zuul)来访问内部服务。网关接收到请求后，从注册中心（Eureka）获取可用服务。由Ribbon进行
均衡负载后，分发到后端的具体实例。微服务之间通过 Feign 进行通信处理业务。业务部署方式相同，都需要前置一个网关来隔绝外部直接调用原子服务
的风险。Dubbo需要自己开发一套API 网关，而Spring Cloud则可以通过Zuul配置即可完成网关定制。使用方式上Spring Cloud略胜一筹。<br/>