

## Cglib动态代理



定义拦截器，去拦截一个类中的所有方法

~~~java
class MyInterceptor0 implements MethodInterceptor{

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("增强0");
        Object o1 = methodProxy.invokeSuper(o, objects);
        return o1;
    }
}
~~~

利用拦截器去创建代理对象

~~~java
class Test{
    public Object getProxyInstance(){
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(A.class);
        enhancer.setCallback(new MyInterceptor());
        return enhancer.create();
    }
}
~~~



#### 针对不同方法做不同处理

定义过滤器把不同方法映射到不同拦截器上

~~~java
class MyCallackFilter implements CallbackFilter {

    // 返回值表示顺序
    @Override
    public int accept(Method method) {
        if ("show".equals(method.getName())){
            System.out.println("拦截到了"+method.getName());
            return 0;
        }
        return 1;
    }
}
~~~

返回值表示使用拦截器的顺序



所以还需要定义多个拦截器

~~~java
class MyInterceptor0 implements MethodInterceptor{

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("增强0");
        Object o1 = methodProxy.invokeSuper(o, objects);
        return o1;
    }
}
class MyInterceptor1 implements MethodInterceptor{

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("增强1");
        Object o1 = methodProxy.invokeSuper(o, objects);
        return o1;
    }
}
~~~

把拦截器和过滤器都设置到增强器中

~~~java
class Test{
    public Object getProxyInstance(){
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(A.class);
        enhancer.setCallbacks(new Callback[]{new MyInterceptor0(),new MyInterceptor1()});
        enhancer.setCallbackFilter(new MyCallackFilter());
        return enhancer.create();
    }
}
~~~



**可以把获取实例的方法直接放在拦截器中**





## 异步方法

用Java8自带的CompletableFuture就可以实现异步地调用

定义一个异步方法，返回值使用CompletableFuture<T>。可以带返回值也可以不带。也可以传入自己的线程池。

~~~java
// 这里定义了一个带返回值的
CompletableFuture<String> precess(String task, Executor executor){
        return CompletableFuture.supplyAsync(()->{
            System.out.println(Thread.currentThread()+" 执行Process1");
            return task + " 执行Process1";
        },executor);
    }
~~~

可以通过CompletableFuture，来实现任务之间的调用

~~~java
public void asyncTest(){
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        // process1完成后，process2又会异步地执行
        CompletableFuture<String> res = process1.precess("测试开始", executorService).thenCompose(a -> {
            return process2.precess("测试开始", executorService);
        });
        try {
            System.out.println(res.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

    }
~~~

也可以直接通过同步的执行每个异步任务的get方法，来实现同步执行。





## 定时任务

利用线程池相关API

Executors.newScheduledThreadPool()创建的ScheduledExecutorService类型周期执行器。ScheduledExecutorService继承了ExecutorService并且多提供了几个周期相关的执行方法。

- schedule()
  - 参数为任务、时间、单位，可以将任务延后执行
- scheduleWithFixedDelay()
  - 参数为为任务、初次延时时间、延时时间、单位，表示任务第一次执行的时间延时，并且当任务执行完毕后，延时固定时间，再次执行任务。
- scheduleAtFixedRate()
  - 参数为为任务、初次延时时间、延时时间、单位，表示任务第一次执行时间延时，当任务开始时就计时固定时间，再次执行任务。如果前一个任务没有完成，就等任务完成后，立刻执行下一个任务。







