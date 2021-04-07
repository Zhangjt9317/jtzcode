---
title: RPC实战与核心原理
date: 2021-04-03 21:56:02
tags:
---

## RPC 实战与核心原理

RPC 通行流程解释，没有 RPC 框架，如何调用另一台服务器上的接口呢？

RPC 全称式 Remote Procedure Call，远程过程调用，跨机器用网络编程才能实现，但是不是只要通过网络通信访问到另一个机器的应用程序，就可以叫 RPC 调用了？显然不够

RPC 是帮助我们屏蔽网络编程细节，实现调用远程方法就跟调用本地一样的体验，我们不需要因为这个方法是远程调用就需要编写很多与业务无关的代码。

这就好比建在小河上的桥一样连接着河的两岸，如果没有小桥，我们需要通过划船、绕道等其他方式才能到达对面，但是有了小桥之后，我们就能像在路面上一样行走到达对面，并且跟在路面上行走的体验没有区别。所以我认为，RPC 的作用就是体现在这样两个方面：

- 屏蔽远程调用跟本地调用的区别，让我们感觉就是调用项目内的方法；
- 隐藏底层网络通信的复杂性，让我们更专注于业务逻辑

一个完整的 RPC 会涉及哪些步骤呢？

我们已经知道了 RPC 是一个远程调用，那就肯定需要通过网络来传输数据，并且 RPC 常用于业务系统之间的数据交换，需要保证其可靠性，所以 RPC 一般默认采用 TCP 来传输。常用的 HTTP 协议也是构建在 TCP 之上的。

网络传输必须是二进制数据，但调用方请求的出入参数都是对象。对象是肯定没法直接在网络中传输的，需要提前把它转成可传输的二进制，并且要求转换算法是可逆的，这个过程我们一般叫做“序列化”。

调用方持续的把请求参数序列化成二进制后，通过 TCP 传输给了服务提供方。服务提供方从 TCP 通道里面收到二进制数据，那如何知道一个请求的数据到哪里结束，是一个怎么类型的请求呢？

根据协议格式，服务提供方就可以正确的从二进制数据中分割出不同的请求来，同时根据请求类型和序列化类型把二进制的消息体逆向还原成请求对象，过程叫做“反序列化”。

服务提供方再更具反序列化出来的请求对象来找到对应的实现类，完成真正的方法调用，然后把执行结果序列化后，回写到对应的 TCP 通道里面。调用方获取到应答的数据包后，再反序列化成应答对象，这样就完成了一次 RPC 调用。

那上述几个流程就组成了一个完整的 RPC 吗？

还缺点东西。需要简化 API（太多的 RCP 底层细节，需要手写代码去构造对象，调用序列化，并且进行网络调用，整个 API 非常不友好）。

Spring 中的 AOP 技术核心采用动态代理的技术，通过字节码增强对方法进行拦截增强，以便于增加需要的额外处理逻辑。其实这个技术也可以应用到 RPC 场景来解决我们刚才面临的问题。

![image](https://static001.geekbang.org/resource/image/ac/fa/acf53138659f4982bbef02acdd30f1fa.jpg)

**RPC 在家沟中的位置**

围绕 RPC 我们讲了这么多，那 RPC 在架构中究竟处于什么位置呢？

RPC 是解决应用间通信的一种方式，而无论是在一个大型的分布式应用系统还是中小型系统中，应用架构最终都是会从“单体”衍化成“微服务化”，整个应用西永会被拆分成多个不同功能的应用，并将它们部署在不同的服务器中，而应用之间会通过 RPC 进行通信，可以说 RPC 对应的是整个分布式系统。

那么如果没有 RPC，我们现实中的开发过程是怎样的一个体验呢？


---> 所有的功能代码都会被我们堆砌在一个大项目中，开发过程中你可能要改一行代码，但改完后编译会花掉你 2 分钟

---> 编译完想运行起来验证下结果可能要 5 分钟，是不是很酸爽？

---> 更难受的是在人数比较多的团队里面，多人协同开发的时候，如果团队其他人把接口定义改了，你连编译通过的机会都没有，系统直接报错，从而导致整个团队的开发效率都会非常低下。

---> 而且当我们准备要上线发版本的时候，QA 也很难评估这次的测试范围，为了保险起见我们只能把所有的功能进行回归测试，这样会导致我们上线新功能的整体周期都特别长

如何解决这个问题呢？首先使用“分而治之”的思想来拆分，但是拆分完的系统怎么保持跟未拆分前的调用方式一样呢？总不能所有的代码都推到重写一遍吧

RPC框架可以帮助我们解决系统拆分后的通信问题，并且能让我们像调用本地一样去调用远程方法。利用RPC我们可以很方便的将应用架构从“单体”演进成“微服务化”，而且还能解决实际开发中效率低下，系统耦合等问题，这样我们的系统架构整体会更加清晰，健壮，应用可运维度增强。

当然RPC还能用在其他场景，比如，发MQ， 发布式缓存，数据库等。

![image](https://static001.geekbang.org/resource/image/50/be/506e902e06e91663334672c29bfbc2be.jpg)

在这个应用中，使用了 MQ 来处理异步流程、Redis 缓存热点数据、MySQL 持久化数据，还有就是在系统中调用另外一个业务系统的接口，对我的应用来说这些都是属于 RPC 调用，而 MQ、MySQL 持久化的数据也会存在于一个分布式文件系统中，他们之间的调用也是需要用 RPC 来完成数据交互的。

一些问题：
* 调用过程中超时了怎么处理业务？
* 什么场景下最适合使用RPC？
* 什么时候才需要考虑开启压缩？

1. 你应用中那些地方用到了RPC
2. RPC使用过程中需要注意什么？

### 02 | 协议：怎么设计可扩展且向后兼容的协议

RPC 核心原理是我们想调用本地一样调用远程，帮助我们的应用层屏蔽远程调用的复杂性，使得我们可以更加方便地构建分布式系统，总结起来就是：透明化

接着聊聊 RPC 协议

![u](https://static001.geekbang.org/resource/image/5c/99/5ca698cbdc61b8e8b090773406b3ab99.jpg)

### 03 | 序列化： 对象怎么在网络中传输？

序列化就是将对象转换成二进制数据的过程，而反序列就是反过来将二进制转换为对象的过程

为何RPC框架需要序列化？ --> 网络传输数据必须是二进制

![RPC 通信流程图](https://static001.geekbang.org/resource/image/82/59/826a6da653c4093f3dc3f0a833915259.jpg)


#### 常见的序列化？

1. JDK 原生序列化
   1. 如果你使用Java语言开发，你一定知道JDK原生的序列化，下面是JDK序列化的一个例子

    ```Java
    import java.io.*;

    public class Student implements Serializable {
        //学号
        private int no;
        //姓名
        private String name;

        public int getNo() {
            return no;
        }

        public void setNo(int no) {
            this.no = no;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return "Student{" +
                    "no=" + no +
                    ", name='" + name + '\'' +
                    '}';
        }

        public static void main(String[] args) throws IOException, ClassNotFoundException {
            String home = System.getProperty("user.home");
            String basePath = home + "/Desktop";
            FileOutputStream fos = new FileOutputStream(basePath + "student.dat");
            Student student = new Student();
            student.setNo(100);
            student.setName("TEST_STUDENT");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            oos.writeObject(student);
            oos.flush();
            oos.close();

            FileInputStream fis = new FileInputStream(basePath + "student.dat");
            ObjectInputStream ois = new ObjectInputStream(fis);
            Student deStudent = (Student) ois.readObject();
            ois.close();

            System.out.println(deStudent);

        }
    }
    ```
JDK自带的序列化机制对使用者而言是很简单的。序列化具体的实现式由ObjectOutputStream完成的，而反序列化的具体实现是由ObjectInputStream完成的

![ObjectOutputStream序列化过程图](https://static001.geekbang.org/resource/image/7e/9f/7e2616937e3bc5323faf3ba4c09d739f.jpg)

实际上任何一种序列化框架，核心思想就是设计一种序列化协议，将对象的类型，属性类型，属性值按照固定的格式写到二进制字节流中来完成序列化，再按照固定的格式 -- 读出对象的类型，属性类型，属性值，通过这些信息创建出一个新的对象，来完成反序列化。

2. JSON
   1. 典型的Key-Value方式，没有数据类型，是一种文本型序列化框架，用在前台Web用Ajax调用，用磁盘存储文本类型的数据，还是基于HTTP协议的RPC框架通信，都会选择JSON格式。
   2. 两个问题：
      1. JSON 进行序列化的额外空间开销比较大，对于大数据量服务这意味着需要巨大的内存和磁盘开销；
      2. JSON 没有类型，但像 Java 这种强类型语言，需要通过反射统一解决，所以性能不会太好。
   3. 所以如果 RPC 框架选用 JSON 序列化，服务提供者与服务调用者之间传输的数据量要相对较小，否则将严重影响性能。
3. Hessian
   1. 动态类型，二进制，紧凑的，可跨语言移植的一种序列化框架，比JDK，JSON更加紧凑，性能上要比JDK，JSON序列化高校很多，而且生成的字节数也更小

```

Student student = new Student();
student.setNo(101);
student.setName("HESSIAN");

//把student对象转化为byte数组
ByteArrayOutputStream bos = new ByteArrayOutputStream();
Hessian2Output output = new Hessian2Output(bos);
output.writeObject(student);
output.flushBuffer();
byte[] data = bos.toByteArray();
bos.close();

//把刚才序列化出来的byte数组转化为student对象
ByteArrayInputStream bis = new ByteArrayInputStream(data);
Hessian2Input input = new Hessian2Input(bis);
Student deStudent = (Student) input.readObject();
input.close();

System.out.println(deStudent);
```

4. Protobuf

* 序列化后体积比JSON, Hessian要小很多
* IDL能清晰地描述含义，所以足以帮助与保证应用程序之间的类型不会丢失，无需类似XML解析器
* 序列化和反序列化速度很快，不需要通过反射获取类型
* 消息格式升级和兼容性不错，可以做到向后兼容


```

/**
 *
 * // IDl 文件格式
 * synax = "proto3";
 * option java_package = "com.test";
 * option java_outer_classname = "StudentProtobuf";
 *
 * message StudentMsg {
 * //序号
 * int32 no = 1;
 * //姓名
 * string name = 2;
 * }
 * 
 */
 
StudentProtobuf.StudentMsg.Builder builder = StudentProtobuf.StudentMsg.newBuilder();
builder.setNo(103);
builder.setName("protobuf");

//把student对象转化为byte数组
StudentProtobuf.StudentMsg msg = builder.build();
byte[] data = msg.toByteArray();

//把刚才序列化出来的byte数组转化为student对象
StudentProtobuf.StudentMsg deStudent = StudentProtobuf.StudentMsg.parseFrom(data);

System.out.println(deStudent);
```
Protobuf很高效，但是对于具有反射和动态能力的语言来说，这样用起来很费劲，这一点不如Hessian，比如用Java的话，这个与编译的过程不是必须的，可以考虑使用Protostuff 


如何选择序列化协议，要考虑到通用性和兼容性，如下图

![优先级](https://static001.geekbang.org/resource/image/b4/a5/b42e44968c3fdcdfe2acf96377f5b2a5.jpg)

中和考虑上述因素，再总结一下这几个序列化协议

我们首选的还是 Hessian 与 Protobuf，因为他们在性能、时间开销、空间开销、通用性、兼容性和安全性上，都满足了我们的要求。其中 Hessian 在使用上更加方便，在对象的兼容性上更好；Protobuf 则更加高效，通用性上更有优势。



### 04 | 网络通信：RPC框架在网络通信上更加倾向于哪种网络IO模型？

#### 常见的网络IO模型

提到网络通信，就会说到网络IO模型，为什么要讲网络IO模型呢？因为所谓的两台PC机之间的网络通信，实际上就是两台PC机对网络IO的操作。

常见的IO模型分为四种：
1. 同步阻塞IO （BIO)
2. 同步非阻塞IO (NIO)
3. IO多路复用 
4. 异步非阻塞IO (AIO)

只有AIO为异步IO，其他都是同步IO,其中用的最多的是同步阻塞IO和IO多路复用

