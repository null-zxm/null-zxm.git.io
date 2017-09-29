#####并行模式和算法

* 聊聊单例模式

	单例模式可以确保系统中一个类只能生成一个实例。优点就是节省new对象的时间，减小堆的压力，以及GC，缩短GC的停顿。

	简单实现：

    	//简单单例模式
		public class Singleton1 {
    	private Singleton1(){
        	System.out.println("Singleton1 is Create");
   		 };
   		private static Singleton1 instant =new Singleton1();
    		public static Singleton1 getSingleton1(){
        		return instant;
    		}
		}

	这个单例的性能是非常好的 可在多线程下面跑起来 但是美中不足的是 Singleton1 实例的创建时间不能控制，类变量在初始化阶段就已经创建了。

 	延时加载的实现

   		public class Singleton2 {
    		private Singleton2(){
				System.out.println("Singleton2 is Create");
    		}
    		private static Singleton2 instance =null;
   			public static Singleton2 getInstance(){
       	 		if (instance==null){
           			 return new Singleton2();
        		}
       	 		return instance;
    		}
		}

	上面这个实现是一种延时加载的实现，也就是可以需要的时候再创建，缺点就是，在多线程下跑起来会出现问题，啥问题应该都知道，解决这个问题可以在getInstance()这个方法使用同步加锁，为了优化还能够使用双重检查模式，但是这种有点丑陋这里就不写出来了，而且在jdk低版本还不一定能正确，这里不推荐使用。下面实现一种高效并且能在多线程中跑起来的延时加载单例

		public class Singleton3 {
  		  private Singleton3(){
      	  System.out.println("Singleton3 is Create");
   		 }
   		 private static class SingletonHander{
       		 private static Singleton3 instance = new Singleton3();
   		 }
   		 public static Singleton3 getinstance(){
       		 return SingletonHander.instance;
    		}
		}
    这里巧妙的利用了内部类初始化的时机，这里可以去看看我写的虚拟机类加载机制这篇博客

* 不变模式

	不变模式的主要应用场景需要满足2个条件

	1.当对象创建之后，其内部状态和数据不再发生任何变化

	2.对象需要被共享，被多线程频繁的访问。

	在java中，不变模式的实现很简单只需要注意下面4点

	1.取出Setter方法以及改变自身属性的所有方法
	
	2.将所有的属性设置为私有，并且用final修饰

	3.确保没有子类可以重载修改他的行为

	4.有一个可以创建完整对象实例的构造函数

	
* 生产者消费者模式

	这是一个很经典的多线程设计模式。在生产者消费者模式中，通常有两种线程，若干个生产者线程，若干个消费者线程， 生产者线程负责提供请求，消费者线程负责处理请求，消费者并不直接和生产者通信，而是通过共享的内存缓冲区域，实现解耦，并行处理。

	共享的内存缓冲区域是这个模式的核心，要使其能够在并发的情况下正确的发生，那么这个容器必须要实现线程之间的同步，如BlockigQueue 使用了锁和阻塞达成同步，还能够使用ConcurrentLinkedQueue，这是一个高性能的，因为它使用了CAS 无锁操作。下面来说说一个超级屌的框架，Disruptor，号称最快的框架。
	
	* Disruptor-无锁的缓存框架
	
	它使用了无锁的方式实现了一个环形队列，在Disrupot中，使用了一种环形队列（RingBuffer）来代替普通的同步队列，这个环形队列内部实现了一个普通的数组。

	一般的队列需要提供队列同步的head和tail两个指针，用于出队和入队，这样子无疑增加了线程协作的复杂度，但是如果队列是环的，就只要向外提供一个当前位置cursor就可以了，利用cursor就可以实现出队和入队操作了。因为是环状的，所以size必须要确定，不能动态的扩展，为了能够快速从一个sequence对应到数组的实际位置，Disruptor要求我们size必须要是2的整数次幂这样就能通过sequence&(size-1)就能够快速定位到index。

	提示：sequence&(size-1)比%快的原因是size的2进制必然为10，100，1000，10000，那么size-1就全都是1，这个时候可以将sequence限定在size-1的范围内，并且不会有一位浪费。当然 为了防止多个消费者处理同一个数据，使用CAS操作来保护数据。

* Future模式

	Future模式是在多线程开发中经常使用的模式，它的核心思想就是异步调用，和我们网上购物一样，下单 付钱了，就可以做自己的事了，等到到了去签收就好了。通常应用在调用一个比较耗时的程序的时候应用。

	简单实现Future

		public interface Data {
    		public String getResult();
		}

		public class FutureData implements Data {
   			 protected RealData realData;
    		 protected boolean isReady;
   			 @Override
   			 public synchronized String getResult() {
       			 if(!isReady){
           			 try {
               			 wait();
           			 } catch (InterruptedException e) {
               			 e.printStackTrace();
           			 }
       			 }
       			 return realData.getResult();
    		}
   			 public synchronized void setRealData(RealData realData){
        		if(isReady){
          		  return;
      			  }
       			 this.realData=realData;
        		 isReady=true;
       			 notifyAll();
    			}
			}

			public class RealData implements Data {
    			protected final String result;
    			public RealData(String result){
       			 try {
            			Thread.sleep(1000);//模拟
       			 } catch (InterruptedException e) {
           			 e.printStackTrace();
        			}
       			 this.result=result;
   			 }
   			 @Override
   			 public String getResult() {
       			 return result;
    			}
			}

		public class Client {
  			  public Data request(final String queryStr){
      			  final FutureData futureData= new FutureData();
      			  new Thread(){
      			      @Override
      			      public void run() {
      			          RealData realData = new RealData(queryStr);
      			          futureData.setRealData(realData);
      			      }
        			}.start();
        			return futureData;
    			}

   			 public static void main(String[] args) {
        		 Client client = new Client();
       			 Data data=client.request("hello");
       			 System.out.println("The request is finish");
        		 System.out.println("Working ...");
        		
				try {
            		Thread.sleep(10000);
       		 } catch (InterruptedException e) {
           		 e.printStackTrace();
       		 }
       		 System.out.println("The Data is "+data.getResult());
   		 }
		}

* 流水线

	并发算法虽然可以充分发挥CPU的性能，不幸的是，并不是所有的计算可以改造成并发的形式，例如（a+b）* b/2 这个计算必须要先计算（a+b）之后才能计算后面，这个时候就要使用流水线思想了。

	简单Demo
	
		public class Msg {
		    public String data;
		}

		public class Dosome1 implements Runnable {
		    public static BlockingQueue<Msg> msgs= new LinkedBlockingQueue<>();
		    @Override
		    public void run() {
		        while(true) {
		            try {
		                Msg msg = msgs.take();
		                System.out.println("Do something");
		                System.out.println(msg.data = "do first" + msg.data);
		                Dosome2.msgs.add(msg);
		            } catch (InterruptedException e) {
		                e.printStackTrace();
		            }
		        }
		    }
		}
		
		public class Dosome2 implements Runnable {
		    public static BlockingQueue<Msg> msgs= new LinkedBlockingQueue<>();
		    @Override
		    public void run() {
		        while(true) {
		            try {
		                Msg msg = msgs.take();
		                System.out.println("Do something");
		                System.out.println(msg.data = "do second" + msg.data);
		            } catch (InterruptedException e) {
		                e.printStackTrace();
		            }
		        }
		    }
		}

		public class Main {
		    public static void main(String[] args) {
		        new Thread(new Dosome1()).start();
		        new Thread(new Dosome2()).start();
		        for (int i=0;i<100;i++){
		            Msg msg= new Msg();
		            msg.data="第"+i+"个数据";
		            Dosome1.msgs.add(msg);
		        }
		    }
		}

