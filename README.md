# RPC_Example

RPC（Remote Procedure Call）叫作远程过程调用，它是利用网络从远程计算机上请求服务，可以理解为把程序的一部分放在其他远程计算机上执行。通过网络通信将调用请求发送至远程计算机后，利用远程计算机的系统资源执行这部分程序，最终返回远程计算机上的执行结果。

RPC的五个主要部分

- user（服务调用方）
- user-stub（调用方的本地存根）
- RPCRuntime（RPC通信者）
- server-stub（服务端的本地存根）
- server（服务端）

服务调用方、调用方的本地存根及其一个RPC通信包的实例存在于调用者的机器上；而服务提供方、服务提供方的存根及另一个RPC通信包的实例存在于被调用的机器上。



服务方的代码启动后要能够接受调用方的网络请求（如Netty、Tomcat）

从一个简单的案例分析

- Provider模块为服务方，提供服务（接口）的实现，接收调用方的调用（网络请求），并返回执行结果
- Consumer模块为调用方，发送请求给服务方，并接受执行结果
- Common模块为公共模块，提供服务（接口）的定义。供Provider和Consumer使用
- RPC模块为RPC架构的实现框架
    - 提供网络服务的启动（HttpServer）
    - 提供网络请求的处理（HttpServerHandler）
    - 提供接口名和实现类的映射，便于快速找到网络请求调用方法对应的实现类（LocalRegister，这里使用本地注册）
    - 提供网络请求中接口名，方法名，参数类型数组，参数数组的封装（Invocation，需要实现序列化接口）
    - 提供代理工厂类用于Consumer创建代理对象，通过代理对象直接调用服务中的方法（ProxyFactory，通过JDK中的动态代理类Proxy实现）