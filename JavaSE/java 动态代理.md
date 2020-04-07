# JAVA动态代理

## 代理模式

为其它对象提供一个代理以控制对某个对象的访问。代理类主要负责为委托类(真实对象)预处理消息、过滤消息、传递消息给委托类，代理类不实现具体的服务，而是利用委托类来完成服务。  

其实就是代理类为被代理类预处理消息、过滤消息并在此之后将消息转发给被代理类，之后还能进行消息的后置处理。代理类和被代理类通常会存在关联关系(即上面提到的持有被代理对象的引用)，代理类本身不实现服务，而是通过调用被代理类中的方法来提供服务。  

## 静态代理

创建一个接口，然后创建被代理类（实现该接口并且实现该接口中的抽象方法）。之后再创建一个代理类，同时使其也实现这个接口。在代理类中持有一个被代理对象的引用，而后在代理类方法中调用该对象的方法。  

### 接口

``` java
public interface HelloInterface{
    void sayHello();
}
```

### 被代理类

``` java
public class Hello implements HelloInterface {
    @Override
    public void sayHello() {
        System.out.println("Hello Chenqi!")
    }
}
```

### 代理类

``` java
public class HelloProxy implements HelloInterface {
    private HelloInterface helloInterface;
    public HelloProxy(HelloInterface helloInterface) {
        this.helloInterface = helloInterface;
    }
    @Override
    public void sayHello() {
        System.out.println("Before invoke sayHello!");
        helloInterface.sayHello();
        System.out.println("After invoke sayHello!");
    }
}
```

### 代理类调用

被代理类被传递给了代理类HelloProxy, 代理类在执行具体方法时通过所持有的被代理类来完成调用。  

``` java
	public static void main(String[] args) {
        HelloProxy helloProxy = new HelloProxy(new Hello());
        helloProxy.sayHello();
       
    }

输出：
Before invoke sayHello!
Hello Chenqi!
After invoke sayHello!
```

![](https://xiaopohai-1254153894.cos.ap-chengdu.myqcloud.com/xiaopohai-blog/3-1Q115093011523.gif)

## 动态代理

### JDK动态代理

1、利用反射机制在运行时创建代理类

2、接口、被代理类不变，我们构建一个handler类来实现InvocationHandler接口。  

#### 配置代理类

``` java
public class ProxyHandler implements InvocationHandler {
    private Object object;
    public ProxyHandler(Object object) {
        this.object = object;
    }
    @Override
    public Object invoke(Object object, Method method, Object[] args) {
        System.out.println("Before invoke " + method.getName());
        method.invoke(object, args);
        System.out.println("After invoke "+ method.getName());
    }
}
```

#### 执行动态代理

``` java
	public void static void main(String[] args) {
        System.getProperties().setProperty("sun.misc.ProxyGenerator.saveGenerateratedFiles", "true");
        HelloInterface hello = new Hello();
        InvocationHandler handler = new ProxyHandler(hello);
        HelloInterface helloProxy = (HelloInterface)Proxy.newProxyInstance(hello.getClass().getClassLoader(), hello.getClass().getInterfaces(), handler);
        helloProxy.sayHello();
    }

输出：
    Before invoke sayHello
    Hello Chenqi!
    After invoke sayHello
```

通过Proxy类的静态方法newProxyInstance返回一个接口的代理实例。针对不同的代理类，传入相应的代理程序控制器InvocationHandler。  

如果新来一个被代理类Bye,如：  

``` java
public Interface ByeInterface {
    void sayBye();
}

public class Bye implements ByeInterface {
    @Override
    public void sayBye() {
        System.out.println("Bye Chenqi!");
    }
}
```

那么执行过程：

``` java
	public void static void main(String[] args) {
        HelloInterface hello = new Hello();
        ByeInterface bye = new Bye();
        InvocationHandler handler = new ProxyHandler(hello);
        InvocationHandler handler1 = new ProxyHandler1(bye);
        HelloInterface helloProxy = (HelloInterface)Proxy.newProxyInstance(hello.getClass().getClassLoader(), hello.getClass().getInterfaces(), handler);
        ByeInterface byeProxy = (ByeInterface)Proxy.newProxyInstance(bye.getClass().getClassLoader(), bye.getClass().getInterfaces(), handler1);
        proxyHello.sayHello();
        proxyBye.sayBye();
    }
输出：
    Before invoke sayHello
    Hello Chenqi
    After invoke sayHello
    Before invoke sayBye
    Bye Chenqi
    Aafter invoke sayBye
```

#### JDK动态代理低层实现

动态代理具体步骤：

1. 通过InvocationHandler接口创建自己的调用处理器;
2. 通过为Proxy类指定ClassLoader对象和一组interface来创建动态代理类;
3. 通过反射机制获得动态代理类的构造方法，其唯一参数类型是调用处理器接口类型;
4. 通过构造函数来创建动态代理类实例，构造时调用处理器对象作为参数被传入。  

既然生成代理对象时用的Proxy类的静态方法newProxyInstance,那么我们就去它的源码里看一下它到底做了些什么？



// todo: 关于JDK动态代理的源码暂时先不研究，对JAVA的安全管理器和加载器不了解，后期补上

[JAVA动态代理](https://www.jianshu.com/p/9bcac608c714)

[Java动态代理详解](https://www.jianshu.com/p/4dcc74b63f1c)

### CGLIB动态代理

#### 1、maven引入CGLIB包

#### 2、 编写UserDao类

``` java
public class UserDao() {
    public void select() {
        System.out.println("UserDao 查询 selectById")
    }
    public void update() {
        System.out.println("UserDao 更新 update")
    }
}
```

#### 3、编写MethodInterceptor的实现类

``` java
public class LogInterceptor implements MethodInteceptor {
    @Override
    public Object intercept(Object object, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable{
        before();
        Object result = method.invokeSuper(object, objects);  //注意这里是调用invokeSuper,而不是invoke,否则会死循环，methodProxy.invokesuper执行的是原始类的方法，method.invoke执行的是子类的方法
        after();
        return result;
    }
    
    private void before() {
        System.out.println(String.format("log start time [%s]", new Date()));
    }
    
    private void after() {
        System.out.println(String.format("log end time [%s]", new Date()));
    }
}
```

#### 4、 测试

``` java
public CglibTest() {
    public static void main() {
        DaoProxy daoProxy = new DaoProxy();
    }
}
```

