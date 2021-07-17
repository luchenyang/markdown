https://spring.io/projects/spring-cloud

### OpenFeign原理

```java
/*
	Feign注解的几个值的含义

	name: 指定FeignClient的名称，如果项目使用了Ribbon，name属性会作为微服务的名称，用于服务发现
	url: url一般用于调试，可以手动指定@FeignClient调用的地址(这里指的是ip和端口)
	decode404: 当发生http 404错误时，如果该字段位true，会调用decoder进行解码，否则抛出FeignException
	configuration: Feign配置类，可以自定义Feign的Encoder、Decoder、LogLevel、Contract
	fallback: 定义容错的处理类，当调用远程接口失败或超时时，会调用对应接口的容错逻辑，fallback指定的类必须实现@FeignClient标							记的接口，这个是实现熔断的 需要开启熔断 配置文件feign.hystrix.enabled：true
	fallbackFactory: 工厂类，用于生成fallback类示例，通过这个属性我们可以实现每个接口通用的容错逻辑，减少重复的代码
	path: 定义当前FeignClient的统一前缀
*/


//获取Feign的代理对象  2.2.0版本 openfeign
public 　class FeignClientFactoryBean{
public Object getObject() {
        return this.getTarget();
    }

     <T> T getTarget() {
        FeignContext context = (FeignContext)this.applicationContext.getBean(FeignContext.class);
        Builder builder = this.feign(context);
        if (!StringUtils.hasText(this.url)) {
            if (!this.name.startsWith("http")) {
                this.url = "http://" + this.name;
            } else {
                this.url = this.name;
            }

            this.url = this.url + this.cleanPath();
            return this.loadBalance(builder, context, new HardCodedTarget(this.type, this.name, this.url));
        } else {
            if (StringUtils.hasText(this.url) && !this.url.startsWith("http")) {
                this.url = "http://" + this.url;
            }

            String url = this.url + this.cleanPath();
            Client client = (Client)this.getOptional(context, Client.class);
            if (client != null) {
                if (client instanceof LoadBalancerFeignClient) {
                    client = ((LoadBalancerFeignClient)client).getDelegate();
                }

                builder.client(client);
            }

            Targeter targeter = (Targeter)this.get(context, Targeter.class);
            return targeter.target(this, builder, context, new HardCodedTarget(this.type, this.name, url));
        }
    }
}

public class ReflectiveFeign{
public <T> T newInstance(Target<T> target) {
        Map<String, MethodHandler> nameToHandler = this.targetToHandlersByName.apply(target);
        Map<Method, MethodHandler> methodToHandler = new LinkedHashMap();
        List<DefaultMethodHandler> defaultMethodHandlers = new LinkedList();
        Method[] var5 = target.type().getMethods();
        int var6 = var5.length;

        for(int var7 = 0; var7 < var6; ++var7) {
            Method method = var5[var7];
            if (method.getDeclaringClass() != Object.class) {
                if (Util.isDefault(method)) {
                    DefaultMethodHandler handler = new DefaultMethodHandler(method);
                    defaultMethodHandlers.add(handler);
                    methodToHandler.put(method, handler);
                } else {
                    methodToHandler.put(method, nameToHandler.get(Feign.configKey(target.type(), method)));
                }
            }
        }

        InvocationHandler handler = this.factory.create(target, methodToHandler);
        T proxy = Proxy.newProxyInstance(target.type().getClassLoader(), new Class[]{target.type()}, handler);
        Iterator var12 = defaultMethodHandlers.iterator();

        while(var12.hasNext()) {
            DefaultMethodHandler defaultMethodHandler = (DefaultMethodHandler)var12.next();
            defaultMethodHandler.bindTo(proxy);
        }

        return proxy;
    }
}
```

https://cloud.tencent.com/developer/article/1009212

