* **Dubbo SPI特点**<br/>
    (1) 对Dubbo进行扩展，不需要改动Dubbo的源码。<br/>
    (2) 自定义的Dubbo的扩展点实现，是一个普通的Java类，Dubbo没有引入任何Dubbo特有的元素，对代码侵入性几乎为零。<br/>
    (3) 将扩展注册到Dubbo中，只需要在ClassPath中添加配置文件。使用简单。而且不会对现有代码造成影响。符合开闭原则。<br/>
    (4) Dubbo的扩展机制支持IoC,AoP等高级功能。<br/>
    (5) Dubbo的扩展机制能很好的支持第三方IoC容器，默认支持Spring Bean，可自己扩展来支持其他容器，比如Google的Guice。<br/>
    (6) 切换扩展点的实现，只需要在配置文件中修改具体的实现，不需要改代码。使用方便。<br/>
    
* **Dubbo底层架构原理图**<br/>
    ![Dubbo底层架构原理图](/images/分布式/Dubbo底层架构原理图.jpg)