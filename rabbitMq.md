### rabbitMq 四种交换机

- 直连交换机，Direct exchange:带路由功能的交换机，根据routing_key(消息发送的时候需要指定)直接绑定到队列，⼀个交换机也可以通过个routing_key绑定多个队列。  

- 扇形交换机，Fanout exchange:广播消息。  

- 主题交换机，Topic exchange:发送到主题交换机上的消息需要携带指定规则的routing_key，主题交换机会根据这个规则将数据发送到对应的(多个)队列列上。  

- 首部交换机，Headers exchange:⾸部交换机是忽略routing_key的一种路由方式。路由器和交换机路由的规则是通过Headers信息来交换的，这个有点HTTP的Headers。将⼀一个交换机声明成首部交换机，绑定⼀个队列的时候，定义一个Hash的数据结构，消息发送的时候，会携带一组hash数据结构的信息，当Hash的内容匹配上的时候，消息就会被写⼊队列。

![](/Users/lu_cy/Library/Application%20Support/marktext/images/2021-03-15-19-13-06-image.png)



### RabbitMq的队列arguments属性

| 标签                    | Arguments                 | 含义                                                   | 缩写  |
| ----------------------- | ------------------------- | ------------------------------------------------------ | ----- |
| Message TTL             | x-message-ttl             | 队列中所有消息的过期时间                               | TTL   |
| Auto expire             | x-expires                 | 队列生存期(毫秒)内没有被使用就会自动删除               | Exp   |
| Max length              | x-max-length              | 队列的最大条数                                         | Lim   |
| Max length bytes        | x-dead-letter-exchange    | 队列消息的最大字节数,超过的话丢弃队列头部的消息        | Lim B |
| Dead letter exchange    | x-dead-letter-exchange    | 死信交换机                                             | DLX   |
| Dead letter routing key | x-dead-letter-routing-key | 死信路由键                                             | DLK   |
| Maximum priority        | x-max-priority            | 队列支持优先级（值为0-255），优先级越大越优先          | Pri   |
| Lazy mode               | x-queue-mode              | 队列具备两种模式：default和lazy，                      |       |
| Master locator          | x-queue-master-locator    | 将队列设置为主位置模式，确定在节点集群队列主位置的规则 |       |

**注：惰性队列会将接收到的消息直接存入文件系统，而不管是持久化的或者是非持久化的，这样可以减少内存的消耗**



### RabbitMQ整个消息投递的过程：

producer→RabbitMQ broker→exchange→queue→consumer 【其中消息实际是存放在queue中】

消息从producer投递到exchange，会返回一个confirmCallback。无论是否投递成功，都会返回。当ack=true，表示成功，当ack=fase，表示失败
消息从exchange投递到queue，投递失败则会返回一个returnCallback





### rabbitMq的属性配置

- auto，自动确认
  在自动确认模式下，消息发送后即被认为成功投递，不管消费者端是否成功处理本次投递

- manual，手动确认
  消费者收到消息后，手动调用basic.ack/basic.nack/basic.reject后，RabbitMQ收到这些消息后，才认为本次投递成功
  手动确认模式可以使用 prefetch，限制通道上未完成的（“正在进行中的”）发送的数量

- none，不确认
  默认情况下消息消费者是NONE模式，默认所有消息消费成功，会不断的向消费者推送消息。因为rabbitMq认为所有消息都被消费成功，所以队列中不在存有消息，消息存在丢失的危险

**prefetch** 设置限流。限制消费者能处理的未确认消息的最大条数，只有当确认接收设置的prefetch条数消息后才能继续处理队列中的消息。

**autoDelete**  **false** 所以当消费者断开连接时，队列不会被删除，**true** 当消费者断开连接时，队列将会被删除

**durable** 设置为 **false**，则该队列为非持久化队列，若设置成 **true** 时，该队列就为持久化队列

**exclusive** 参数，该参数表示该队列是否为单消费者队列



```xml
<!--通过指定下面的admin信息，当前producer中的exchange和queue会在rabbitmq服务器上自动生成 -->
 <rabbit:admin connection-factory="rabbitConnectionFactory" />

 <rabbit:queue name="queueTest" durable="true" auto-delete="false" exclusive="false" >
   <rabbit:queue-arguments>
     <entry key="x-dead-letter-exchange" value="deadExchange" />
     <entry key="x-dead-letter-routing-key" value="queueTestKey" />
   </rabbit:queue-arguments>
 </rabbit:queue>

 <rabbit:listener-container connection-factory="rabbitConnectionFactory" acknowledge="manual" prefetch="3" >
     <rabbit:listener queues="queueTest" ref="confirmListener" />
     <rabbit:listener queues="deadQueue" ref="deadQueueListener"/>
 </rabbit:listener-container>

```



### RabbitMq声明队列方式

- 使用rabbitAdmin创建

  ```java
  import org.springframework.amqp.AmqpException;
  import org.springframework.amqp.core.Binding;
  import org.springframework.amqp.core.BindingBuilder;
  import org.springframework.amqp.core.DirectExchange;
  import org.springframework.amqp.core.FanoutExchange;
  import org.springframework.amqp.core.Message;
  import org.springframework.amqp.core.MessagePostProcessor;
  import org.springframework.amqp.core.MessageProperties;
  import org.springframework.amqp.core.Queue;
  import org.springframework.amqp.core.TopicExchange;
  import org.springframework.amqp.rabbit.core.RabbitAdmin;
  import org.springframework.amqp.rabbit.core.RabbitTemplate;
  
  public class ApplicationTests {
  	
  	@Autowired
  	private RabbitAdmin rabbitAdmin;
  	
  	@Test
  	public void testAdmin() throws Exception {
  	//第一种  先声明在绑定
  		rabbitAdmin.declareExchange(new DirectExchange("test.direct", false, false));
  		rabbitAdmin.declareExchange(new TopicExchange("test.topic", false, false));
  		rabbitAdmin.declareExchange(new FanoutExchange("test.fanout", false, false));
  		rabbitAdmin.declareQueue(new Queue("test.direct.queue", false));
  		rabbitAdmin.declareQueue(new Queue("test.topic.queue", false));
  		rabbitAdmin.declareQueue(new Queue("test.fanout.queue", false));
  		rabbitAdmin.declareBinding(new Binding("test.direct.queue",
  				Binding.DestinationType.QUEUE,
  				"test.direct", "direct", new HashMap<>()));
  		//第二种：直接绑定
  		rabbitAdmin.declareBinding(
  				BindingBuilder
  				.bind(new Queue("test.topic.queue", false))		//直接创建队列
  				.to(new TopicExchange("test.topic", false, false))	//直接创建交换机 建立关联关系
  				.with("user.#"));	//指定路由Key
  		rabbitAdmin.declareBinding(
  				BindingBuilder
  				.bind(new Queue("test.fanout.queue", false))		
  				.to(new FanoutExchange("test.fanout", false, false)));
  		//清空队列数据
  		rabbitAdmin.purgeQueue("test.topic.queue", false);
  	}
  
  ```

- 使用SpringAMQP去申明(不过使用这种方式声明其实和xml声明一样，如果想要自动创建队列还需要rabbitAdmin)

  ```java
      @Bean
      public TopicExchange exchange001() {
  //      是否持久化，是否自动删除
          return new TopicExchange("topic001", true, false);
      }
      
  //    声明 queue001 队列
      @Bean
      public Queue queue001() {
          return new Queue("queue001", true); //队列持久
      }
      
  //    将上面的交换机和队列绑定
      @Bean
      public Binding binding001() {
          return BindingBuilder.bind(queue001()).to(exchange001()).with("spring.*");
      }
      @Bean
      public TopicExchange exchange002() {
          return new TopicExchange("topic002", true, false);
      }
      @Bean
      public Queue queue002() {
          return new Queue("queue002", true);
      }
      @Bean
      public Binding binding002() {
          return BindingBuilder.bind(queue002()).to(exchange002()).with("rabbit.*");
      }
      @Bean
      public Queue queue003() {
          return new Queue("queue003", true); //队列持久
      }
      @Bean
      public Binding binding003() {
          return BindingBuilder.bind(queue003()).to(exchange001()).with("mq.*");
      }
  
  ```

  

### RabbitMq的rabbitAdmin

RabbitAdmin是Spring AMQP封装的一个对象。 在使用Spring AMQP的时候我们只是配置一个RabbitAdmin对象交给Spring容器做管理。如果没有指定rabbitAdmin但是**autoDeclare**为true，那么spring就会创建一个RabbitAdmin对象。有了RabbitAdmin对象，Spring就会为我们创建队列了。

```xml
 <rabbit:queue name="rest-sysmsg-queue" durable="true" auto-declare="true"/>
```

```java
 public void initialize() {
        if (this.applicationContext == null) {
            this.logger.debug("no ApplicationContext has been set, cannot auto-declare Exchanges, Queues, and Bindings");
        } else {
            this.logger.debug("Initializing declarations");
          //获取交换机信息
            Collection<Exchange> contextExchanges = 
              new LinkedList(this.applicationContext.getBeansOfType(Exchange.class).values());
           //获取队列信息
            Collection<Queue> contextQueues = 
              new LinkedList(this.applicationContext.getBeansOfType(Queue.class).values());
           //获取绑定关系
            Collection<Binding> contextBindings = 
              new LinkedList(this.applicationContext.getBeansOfType(Binding.class).values());
            Collection<DeclarableCustomizer> customizers = 
              this.applicationContext.getBeansOfType(DeclarableCustomizer.class).values();
            this.processDeclarables(contextExchanges, contextQueues, contextBindings);
            Collection<Exchange> exchanges = this.filterDeclarables(contextExchanges, customizers);
            Collection<Queue> queues = this.filterDeclarables(contextQueues, customizers);
            Collection<Binding> bindings = this.filterDeclarables(contextBindings, customizers);
            Iterator var8 = exchanges.iterator();
            while(true) {
                Exchange exchange;
                do {
                    if (!var8.hasNext()) {
                        var8 = queues.iterator();
                        while(true) {
                            Queue queue;
                            do {
                                if (!var8.hasNext()) {
                                    if (exchanges.size() == 0 && queues.size() == 0 && bindings.size() == 0) 
                                    {
                                        this.logger.debug("Nothing to declare");
                                        return;
                                    }
                                  //创建
                                    this.rabbitTemplate.execute((channel) -> {
                                        this.declareExchanges(channel, (Exchange[])exchanges.toArray(
                                          new Exchange[exchanges.size()]));
                                        this.declareQueues(channel, (Queue[])queues.toArray(
                                          new Queue[queues.size()]));
                                        this.declareBindings(channel, (Binding[])bindings.toArray(
                                          new Binding[bindings.size()]));
                                        return null;
                                    });
                                    this.logger.debug("Declarations finished");
                                    return;
                                }
                                queue = (Queue)var8.next();
                            } while(queue.isDurable() && !queue.isAutoDelete() && !queue.isExclusive());

                            if (this.logger.isInfoEnabled()) {
                                this.logger.info("xxxx");
                            }
                        }
                    }
                    exchange = (Exchange)var8.next();
                } while(exchange.isDurable() && !exchange.isAutoDelete());
                if (this.logger.isInfoEnabled()) {
                    this.logger.info("xxxx");
                }
            }
        }
    }
```



### RabbitMq消息确认方式

- channel.basicAck(deliveryTag, multiple);
  consumer处理成功后，通知broker删除队列中的消息，如果设置multiple=true，表示支持批量确认机制以减少网络流量。
  例如：有值为5,6,7,8 deliveryTag的投递
  如果此时channel.basicAck(8, true);则表示前面未确认的5,6,7投递也一起确认处理完毕。
  如果此时channel.basicAck(8, false);则仅表示deliveryTag=8的消息已经成功处理。

- channel.basicNack(deliveryTag, multiple, requeue);
  consumer处理失败后，例如：有值为5,6,7,8 deliveryTag的投递。
  如果channel.basicNack(8, true, true);表示deliveryTag=8之前未确认的消息都处理失败且将这些消息重新放回队列中。
  如果channel.basicNack(8, true, false);表示deliveryTag=8之前未确认的消息都处理失败且将这些消息直接丢弃。
  如果channel.basicNack(8, false, true);表示deliveryTag=8的消息处理失败且将该消息重新放回队列。
  如果channel.basicNack(8, false, false);表示deliveryTag=8的消息处理失败且将该消息直接丢弃。

- channel.basicReject(deliveryTag, requeue);
  相比channel.basicNack，除了没有multiple批量确认机制之外，其他语义完全一样。
  如果channel.basicReject(8, true);表示deliveryTag=8的消息处理失败且将该消息重新放回队列。
  如果channel.basicReject(8, false);表示deliveryTag=8的消息处理失败且将该消息直接丢弃。



### RabbitMq的死信队列

什么是死信呢？官方给出三个说法：

> - 消息被拒绝（basic.reject或basic.nack）并且requeue=false.（requeue 看上面 消息确认方式）
> - 消息TTL过期
> - 队列达到最大长度（队列满了，无法再添加数据到mq中）



### RabbitMq的Comfirm模式和returnCallback 回退模式

```java
  RabbitTemplate template = new RabbitTemplate(connectionFactory());
  template.setMandatory(true);
  /* mandatory：用于设置处理失败消息的方式，直接丢失还是返回给消息生产者
（1）当mandatory标志位设置为true时，如果exchange根据自身类型和消息routingKey无法找到一个合适的queue存储消息，那么broker会调用basic.return方法将消息返还给生产者
（2）当mandatory设置为false时，出现上述情况broker会直接将消息丢弃，回调方法也不会执行
*/
```




https://blog.csdn.net/qq_38181949/article/details/108153856

https://zhuanlan.zhihu.com/p/341496950



### RabbitMq的延迟队列

https://www.cnblogs.com/mfrank/p/11260355.html



### Rabbitmq的pull和push模式





### RabbitMq的顺序消费和可靠性

rabbitmq消息的可靠性依赖于rabbitmq的comfirm和returncallback 模式和消费方的手动确认，以及交换机和队列的消息持久化





