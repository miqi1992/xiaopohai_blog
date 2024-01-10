---
id: a8dcba4e-19d1-4bcb-af46-3d1a51bcdd0a
---

# 背八股文和 DEBUG 源码，差别在哪？
#Omnivore

[Read on Omnivore](https://omnivore.app/me/debug-18cf142adaa)
[Read Original](https://mp.weixin.qq.com/s/ZFMSOTvPGcUUjQD6fh_a1A)


原创  江南一点雨  江南一点雨 _2024-01-10 10:36_ _发表于广东_ 

很多小伙伴知道松哥最近在更 Spring 源码相关的文章和视频，视频现在已经全部录完了，公号后台回复 Spring 有视频详细介绍。

今天我想和大伙聊一些解决问题的思路，就像我在 Spring 视频中所讲，我不仅是想让小伙伴们理解 Spring 源码，看懂 Spring 源码，更是想让小伙伴们掌握 DEBUG 源码的思路和方法，相信各位在学习 Spring 源码视频的时候对此也会有所领悟。

这次刚好是有一个小伙伴在群里问了这样一个问题：

![图片](https://proxy-prod.omnivore-image-cache.app/0x0,seHAMKtwqNYBVKYpoSO9z_jo1PmWEqE4DerJMT-CQveY/https://mmbiz.qpic.cn/sz_mmbiz_jpg/GvtDGKK4uYmeUaguTwOKiaBdcIQqBsWicaDmPAGFcrMfFIR3BbiblGytUN0hJ6CnYWN540iaclWugiaxhjqpkiamMiamw/640?wx_fmt=jpeg&from=appmsg)![图片](https://proxy-prod.omnivore-image-cache.app/0x0,skL_HyeSXETYxpEp400BrBE03Socmpcyd9maanZCeFaY/https://mmbiz.qpic.cn/sz_mmbiz_png/GvtDGKK4uYmeUaguTwOKiaBdcIQqBsWicaoiclZvo2ZFHJYonNJqCzPNGOAeIuGOqm08N0dQQqhMBQm5oTwlJ7icqg/640?wx_fmt=png&from=appmsg)

首先这个小伙伴提的这个问题很好懂：SpringMVC 工作流程是面试八股文中的经典，上面这一套流程看起来是前后端不分时候的工作流程（因为涉及到了页面渲染），现在都流行前后端分离架构，那么前后端分离之后，SpringMVC 工作流程还是这样吗？

如果你懂一点源码分析技巧，这个问题其实可以自己分析去解决，但是如果你只会背八股文，那这个问题就有点棘手了。

> 可能有的小伙伴认为这个并无必要，工作中不会用到，面试直接背八股文就行。但是！！！我觉得这样的分析其实是很有必要的，我们工作不仅仅是要养家糊口，我们也需要获得成就感，很多小伙伴总是感觉自己在公司天天 CRUD，工作没有挑战，是一个不折不扣的 CV 战士，那么现在这样一个思考的机会摆在你面前，你冲不冲？如果通过自己分析源码解决了心中的疑惑，会不会自信心爆棚呢？所以，尝试自己去分析这个问题是有意义的。

## 1\. 知识储备

首先，想要自己 DEBUG 去解决问题，必须要有知识储备。不能啥都不懂，就掌握一点 IDEA 上的 DEBUG 技巧，上来就想解决问题，那无疑是天方夜谭。

对于上面这个问题，我们至少需要如下两个知识储备。

1. HandlerAdapter

首先我们需要明白 HandlerAdapter 的作用，是真真正正的了解，不是背诵八股文那种了解。HandlerAdapter 是一个接口，这个接口中最重要的方法就是 handle 方法。

为什么会有 HandlerAdapter 存在呢？这是因为我们在 SpringMVC 中定义接口的方式有很多种，大家日常开发用的最多的就是通过 `@Controller` 或者 `@RestController` 注解来标记接口，但是这并不是接口唯一的定义方式，我们也可以通过实现 Controller 接口、HttpRequestHandler 接口甚至实现 Servlet 接口来完成接口的定义。

这些不同的接口定义方式，自然就对应了不同的调用方式，所以需要一个适配器，对于框架来说，总是通过调用 HandlerAdapter#handle 方法来调用接口方法，而不同的接口定义方式则需要分别提供各自的 HandlerAdapter。

`public interface HandlerAdapter { boolean supports(Object handler); @Nullable ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception; @Deprecated long getLastModified(HttpServletRequest request, Object handler);}`

我们看到默认的 HandlerAdapter 有如下实现类，基本上每种实现类都对应了一个接口调用方式：

![图片](https://proxy-prod.omnivore-image-cache.app/0x0,sNzXafVMqgxpeSV6ohVPoaoRYjaQoES_5NauvgUye8vs/https://mmbiz.qpic.cn/sz_mmbiz_png/GvtDGKK4uYmeUaguTwOKiaBdcIQqBsWicaSPpwbuQ7BhruCpsdsl0tFMVVSDrNt2qmXUiaaJbY8FvZyVjjDz8GfBA/640?wx_fmt=png&from=appmsg)

HandlerAdapter#handle 方法的返回值是 ModelAndView，也就是按理说每个接口都应该返回一个 ModelAndView，但是有时候我们的接口并不是返回这个，最典型的就是如果我们通过实现 Servlet 接口来定义接口，Servlet 接口中的方法返回值是 void，显然就不是 ModelAndView，那么对于这种情况我们该怎么处理呢？我们不妨来看下 SimpleServletHandlerAdapter，这个适配器专门用来处理通过 Servlet 定义的接口：

`public class SimpleServletHandlerAdapter implements HandlerAdapter { @Override public boolean supports(Object handler) {  return (handler instanceof Servlet); } @Override @Nullable public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler)   throws Exception {  ((Servlet) handler).service(request, response);  return null; } @Override @SuppressWarnings("deprecation") public long getLastModified(HttpServletRequest request, Object handler) {  return -1; }}`

可以看到，这个源码可太简单了，直接把接口类强转为 Servlet 然后进行调用。至于 handle 方法的返回值，直接返回 null 算了。

这是我们需要的知识储备一，如果你懂得上面的内容，大概也就能猜出来，如果前后端分离中接口返回了 JSON，那么执行目标接口的 HandlerAdapter#handle 方法估计也是返回 null。

1. HttpMessageConverter

第二个知识储备就是需要明白在 SpringMVC 中 JSON 的生成、解析是谁来完成的。

SpringMVC 返回 JSON 参数特别方便，接口方法直接返回对象就可以了，系统会自动将之转为 JSON 字符串然后写回去；如果提交的参数是 JSON 字符串，我们也只需要在接口中添加 @RequestBody 注解，这样系统就会自动将 JSON 字符串转为 Java 对象了。

这一切的实现，离不开 HttpMessageConverter。我们先来看看 HttpMessageConverter 接口：

``` java
public interface HttpMessageConverter<T> {    
略。。。 /**  * Read an object of the given type from the given input message, and returns it.  * @param clazz the type of object to return. This type must have previously been passed to the  * {@link #canRead canRead} method of this interface, which must have returned {@code true}.  * @param inputMessage the HTTP input message to read from  * @return the converted object  * @throws IOException in case of I/O errors  * @throws HttpMessageNotReadableException in case of conversion errors  */ T read(Class<? extends T> clazz, HttpInputMessage inputMessage)   throws IOException, HttpMessageNotReadableException; /**  * Write a given object to the given output message.  * @param t the object to write to the output message. The type of this object must have previously been  * passed to the {@link #canWrite canWrite} method of this interface, which must have returned {@code true}.  * @param contentType the content type to use when writing. May be {@code null} to indicate that the  * default content type of the converter must be used. If not {@code null}, this media type must have  * previously been passed to the {@link #canWrite canWrite} method of this interface, which must have  * returned {@code true}.  * @param outputMessage the message to write to  * @throws IOException in case of I/O errors  * @throws HttpMessageNotWritableException in case of conversion errors  */ void write(T t, @Nullable MediaType contentType, HttpOutputMessage outputMessage)   throws IOException, HttpMessageNotWritableException;}
```


这个接口中最重要的就是 read 和 write 方法。其中 read 方法是将请求参数中的 JSON 字符串转为 Java 对象，write 方法是将请求响应中的 Java 对象转为 JSON 字符串。

每一个 JSON 处理工具都会提供自身的 HttpMessageConverter，以 Spring Boot 中的 jackson 为例，它的 HttpMessageConverter 是 MappingJackson2HttpMessageConverter。

好了，有了如上两点知识储备，接下来我们就可以结合 IDEA 中的 DEBUG 技能，快速梳理出问题的答案了。

## 2\. 问题分析

那么这个问题从哪里切入呢？

既然服务端要返回 JSON，就必然调用到 HttpMessageConverter#write 方法，那么我们就写一个返回 JSON 的接口，然后在 MappingJackson2HttpMessageConverter#write 方法上打断点，因为最终要生成 JSON 必然会经过该方法。然后结合 IDEA 中 DEBUG 的方法调用栈，就能大致分析出来。

![图片](https://proxy-prod.omnivore-image-cache.app/0x0,styj6HulbSI6QFyhiskZpB1ITrabolo8aiBhiR7okKbA/https://mmbiz.qpic.cn/sz_mmbiz_png/GvtDGKK4uYmeUaguTwOKiaBdcIQqBsWica1ey75FXQmpy5hUH0xHDtGfZk2Eb7mib4TsEEgibzXn6kEqAqbeicqPh4Q/640?wx_fmt=png&from=appmsg)

从这个方法调用栈我们可以看出来，确实是调用了 HandlerAdapter#handle 方法，从这个位置依次往上，我们就找到了触发 JSON 生成的方法：

`@Nullableprotected ModelAndView invokeHandlerMethod(HttpServletRequest request,  HttpServletResponse response, HandlerMethod handlerMethod) throws Exception { //省略。。。 invocableMethod.invokeAndHandle(webRequest, mavContainer); if (asyncManager.isConcurrentHandlingStarted()) {  return null; } return getModelAndView(mavContainer, modelFactory, webRequest);}`

顺着这个方法的调用栈，我们发现 JSON 的生成是在 `invocableMethod.invokeAndHandle` 方法中被触发的，包括 JSON 的写出都是在这个方法中完成的，这块代码简单，我就不贴图了。

那问题来了，JSON 已经写回去了，现在 handle 方法需要返回 ModelAndView 该怎么办呢？这就是接下来 getModelAndView 方法的作用了：

`@Nullableprivate ModelAndView getModelAndView(ModelAndViewContainer mavContainer,  ModelFactory modelFactory, NativeWebRequest webRequest) throws Exception { modelFactory.updateModel(webRequest, mavContainer); if (mavContainer.isRequestHandled()) {  return null; } //省略。。。}`

这个方法有一句判断 `mavContainer.isRequestHandled()`，看方法名就知道表示检查请求是否已经被处理了，由于前面已经处理完 JSON 了，所以这个方法就返回 true，这就进而导致返回的 ModelAndView 是一个 null。

继续跟进方法调用栈的提示，看接下来的处理。

![图片](https://proxy-prod.omnivore-image-cache.app/0x0,s_sV-mdAsKtJsKl54Bfp2V7QcI62WvQUX0FKVoIbqkSk/https://mmbiz.qpic.cn/sz_mmbiz_png/GvtDGKK4uYmeUaguTwOKiaBdcIQqBsWica5WHSutxOXUb28JYLJALRiaDRS5j1sI68iaic35jUXBUxibgG06A3uWAybw/640?wx_fmt=png&from=appmsg)

在这个位置调用了 HandlerAdapter#handle 方法，该方法返回了 null，解下来该去找试图解析器进行视图渲染了，视图渲染则是在接下来的 processDispatchResult 方法上：

![图片](https://proxy-prod.omnivore-image-cache.app/0x0,sE0D14ygSaM92TWu_GaZirm9Kyv5omgtXV40lEV4jETc/https://mmbiz.qpic.cn/sz_mmbiz_png/GvtDGKK4uYmeUaguTwOKiaBdcIQqBsWicaffIWg4qj5OSib0j65TiahqN8mH5lSQ5mFf8NgHhtD4NWTYQsnbbddGMA/640?wx_fmt=png&from=appmsg)

`private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,  @Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,  @Nullable Exception exception) throws Exception { //省略 // Did the handler return a view to render? if (mv != null && !mv.wasCleared()) {  render(mv, request, response);  if (errorView) {   WebUtils.clearErrorRequestAttributes(request);  } } else {  if (logger.isTraceEnabled()) {   logger.trace("No view rendering, null ModelAndView returned.");  } }    //省略。。。}`

大家可以看到，这里会判断这个 ModelAndView 是否为 null，不为 null 的话，会去调用 render 方法进行视图的渲染，这个时候就会去找到视图解析器，分析视图，渲染视图，这个松哥之前也都和大家聊过了。但我们这里由于 mv 是 null，所以这一步其实是跳过了，也就是没有去找试图解析器也没有去渲染视图了。

现在再回到本文一开始的问题，相信各位心中已经有答案了。

从这个问题的分析中大家也能看出来，单纯的背八股文真的不如自己去读一读源码理解一下，因为八股文只能解决面试问题，对于工作，对于自身技能的提升作用是有限的。

---

**TienChin 视频杀青啦～采用 Spring Boot+Vue3 技术栈，里边会涉及到各种好玩的技术，小伙伴们来和松哥一起做一个完成率超 90% 的项目，戳戳戳这里-->[TienChin 项目配套视频来啦](https://mp.weixin.qq.com/s?%5F%5Fbiz=MzI1NDY0MTkzNQ==&mid=2247503233&idx=1&sn=82c4b49ab1fac4bb4792c0ca869afab2&scene=21#wechat%5Fredirect)。**

---

**Spring 源码分析视频教程连载中，感兴趣的小伙伴戳这里：[Spring源码应该怎么学？](https://mp.weixin.qq.com/s?%5F%5Fbiz=MzI1NDY0MTkzNQ==&mid=2247506361&idx=1&sn=f787d8c7bcc946c9337ab0e20f920bef&scene=21#wechat%5Fredirect)。**