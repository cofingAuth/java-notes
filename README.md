## 系统设计与实现，需要考虑什么？
正确性、健壮性、可靠性、效率、易用性、可读性（可理解性）、可扩展性、可复用性、兼容性、可移植性

## 设计模式
1. 创建型模式
	- 单例模式
	- 工厂方法模式
	- 抽象工厂方法模式
	- 建造者模式
	- 原型模式
2. 结构型模式
	- 适配器模式
	- 装饰者模式
	- 代理模式
	- 外观模式
	- 组合模式
	- 桥接模式
	- 享元模式
3. 行为型模式
	- 策略模式
	- 模板方法模式
	- 观察者模式
	- 责任链模式
	- 命令模式
	- 访问者模式
	- 状态模式
	- 备忘录模式
	- 中介者模式
	- 迭代器模式
	- 解释器模式

### 1.1 单例模式
特点：一个类只能有一个实例，提供一个全局的访问点  
写法：饿汉式、懒汉式、注册式、ThreadLocal属于同一线程单例
#### 饿汉式
	/**
	 * 单例模式：饿汉式
	 * @advantage 线程安全
	 * @disadvantage 在数量不可控/整个业务系统上用的几率低的情况下，性能低、浪费资源
	 */
	public class HungrySingle {
	    private static final HungrySingle hungrySingle = new HungrySingle();

	    private HungrySingle() {}
	
	    public static HungrySingle getInstance(){
	        return hungrySingle;
	    }
	}
	
	/**
	 * 自我感觉是装逼的
	 */
	public class HungrySingleB {
	    private static HungrySingleB hungrySingleB;
	
	    private HungrySingleB() {
	        hungrySingleB = new HungrySingleB ();
	    }
	
	    public static HungrySingleB getInstance(){
	        return hungrySingleB;
	    }
	}
#### 懒汉式
	/**
	 * 单例模式：懒汉式
	 * @advantage 节约资源
	 * @disadvantage 非线程安全
	 */
	public class LazySimpleSingle {
	    private static LazySimpleSingle lazySimpleSingle;
	
	    private LazySimpleSingle(){}
	
	    public static LazySimpleSingle getInstance(){
	        if(lazySimpleSingle == null){
	            lazySimpleSingle = new LazySimpleSingle();
	        }
	        return lazySimpleSingle;
	    }
	}
	
	/**
	 * 单例模式：懒汉式
	 * @advantage 节约资源、线程安全
	 * @disadvantage 性能低
	 */
	public class LazySyncSingle {
	    private static LazySyncSingle lazySyncSingle;
	
	    private LazySyncSingle(){}
	
	    public static synchronized LazySyncSingle getInstance(){
	        if(lazySyncSingle == null){
	            lazySyncSingle = new LazySyncSingle ();
	        }
	        return lazySyncSingle;
	    }
	}

	/**
	 * 单例模式：懒汉式
	 * @advantage 节约资源、线程安全、性能高
	 * @disadvantage 可读性低
	 */
	public class LazyDoubleCheckSingle {
	    private volatile static LazyDoubleCheckSingle lazyDoubleCheckSingle;
	
	    private LazyDoubleCheckSingle(){}
	
	    public static LazyDoubleCheckSingle getInstance(){
	        if(lazyDoubleCheckSingle == null){
	            synchronized (LazyDoubleCheckSingle.class){
	                if(lazyDoubleCheckSingle == null){
	                    lazyDoubleCheckSingle = new LazyDoubleCheckSingle ();
	                }
	            }
	        }
	        return lazyDoubleCheckSingle;
	    }
	}

	/**
	 * 单例模式：懒汉式
	 * @advantage 节约资源、线程安全、性能高、可读性高
	 * @disadvantage 会被反射实例化破坏
	 */
	public class LazyInnerSingle {
	    private LazyInnerSingle(){}
	
	    private static class LazyInnerHolder {
	        private static LazyInnerSingle lazyInnerSingle = new LazyInnerSingle();
	    }
	
	    public static LazyInnerSingle getInstance(){
	        return LazyInnerHolder.lazyInnerSingle;
	    }
	}
#### 注册式	
	/**
	 * 单例模式：注册式
	 * @advantage 线程安全、不会被反射实例化破坏
	 * @disadvantage 回归到饿汉式的缺点
	 */
	public enum EnumRegister {
	    /**
	     * 单例
	     */
	    single;
	}

	/**
	 * 单例模式：容器式单例，应用于spring ioc容器
	 * @advantage 线程安全、不会被反射实例化破坏、节约资源
	 */
	public class ContainerSingle {
	    private ContainerSingle(){}
	    private static final Map<String, Object> singletonObjects = new ConcurrentHashMap<> ();
	
	    public static void registerSingleton(String beanName, Object singletonObject){
	        synchronized (singletonObjects){
	            Object oldObject = singletonObjects.get (beanName);
	            if(oldObject != null){
	                throw new IllegalStateException (beanName + " 已注册");
	            }
	            singletonObjects.put (beanName, singletonObject);
	        }
	    }
	
	    public static Object getInstance(String beanName){
	        return singletonObjects.get (beanName);
	    }
	}
#### ThreadLocal
	/**
	 * 相对单例模式：ThreadLocal
	 * 特点：不能保证整一个程序唯一，可以保证当前线程唯一  
	 * @advantage 不用加锁保证线程安全，在并发竞争大的情况下性能高于sync，以空间换取时间
	 * @disadvantage 不属于仅且有一个实例
	 */
	public class TheadLocalSingle {
	    private static ThreadLocal<TheadLocalSingle> threadLocal = new ThreadLocal<TheadLocalSingle> (){
	        @Override
	        protected TheadLocalSingle initialValue(){
	            return new TheadLocalSingle();
	        }
	    };
	
	    private TheadLocalSingle(){}
	
	    public static TheadLocalSingle getInstance(){
	        return threadLocal.get();
	    }
	}
