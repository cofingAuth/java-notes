## 系统设计与实现，需要考虑什么？
正确性、健壮性、可靠性、效率、易用性、可读性（可理解性）、可扩展性、可复用性、兼容性、可移植性

## 设计模式六大原则
单一职责原则、里氏替换原则、依赖倒置原则、接口隔离原则、迪米特法则、开闭原则

## 设计模式
1. 创建型模式
	- 单例模式
	- 工厂方法模式
	- 抽象工厂模式
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

### 1.2 工厂模式
分类：简单工厂、工厂方法、抽象工厂模式
#### 简单工厂
特点：一个工厂类根据传入的参量决定创建出那一种产品类的实例  
情况：例如游戏控制类，如果游戏控制类引入游戏工厂类，则新增游戏后，游戏控制类不需要修改，只需要修改游戏工厂类；
优点：使用对象与创建实例的分离，实现解耦
缺点：违背开闭原则，如新增游戏，则需要修改游戏工厂；
	
	/**
	 * 游戏抽象类
	 */
	public abstract class Game {
	    public String name;
	
	    abstract void open();
	
	    abstract void close();
	}
	
	/**
	 * NBA游戏
	 */
	public class NBAGame extends Game{
	    public NBAGame() {
	        super.name = "NBA";
	    }
	
	    @Override
	    void open() {
	        System.out.println(super.name + " open");
	    }
	
	    @Override
	    void close() {
	        System.out.println(super.name + " close");
	    }
	}

	/**
	 * 王者荣耀游戏
	 */
	public class WzryGame extends Game{
	    public WzryGame() {
	        super.name = "wzry";
	    }
	
	    @Override
	    void open() {
	        System.out.println(super.name + " open");
	    }
	
	    @Override
	    void close() {
	        System.out.println(super.name + " close");
	    }
	}

	/**
	 * 简单工厂类
	 */
	public class SimpleFactory {
	    public static Game getGame(String gameName){
	        if("wzry".equalsIgnoreCase (gameName)){
	            return new WzryGame ();
	        }else if("NBA".equalsIgnoreCase (gameName)){
	            return new NBAGame ();
	        }
	        return null;
	    }
	}

#### 工厂方法模式
特点：定义一个创建对象的接口，让子类决定实例化那个类  
使用情况：可以参考spring的AbstractBeanFactory抽象类  
优点：更符合开闭原则；如新增游戏，新增具体游戏与相应工厂；

	public abstract class AbstractFactoryGame {
	    public Game getGame(){
	        return createGame();
	    };
	
	    public abstract Game createGame();
	}

	public class NBAFactoryGame extends AbstractFactoryGame{
	    @Override
	    public Game createGame() {
	        return new NBAGame ();
	    }
	}


	/**
	 * spring的AbstractBeanFactory抽象类，他的实现类很多，可以查看ListFactoryBean
	 */
	public final T getObject() throws Exception {
        //单例，从缓存获取，或暴露早期实例，以解决循环引用
        if (isSingleton()) {
            return (this.initialized ? this.singletonInstance : getEarlySingletonInstance());
        }
        else {
            return createInstance();
        }
    }

    //创建对象
    protected abstract T createInstance() throws Exception;

#### 抽象工厂模式
特点：创建相关或依赖对象的家族，而无需明确指定具体类  
优点：分离接口与实现；使切换产品族变得容易  
缺点：扩展影响大，如修改工厂则需要同时修改所有工厂实现类  
情况：系统选择天美游戏或者选择光子游戏其中一种，则可以使用抽象工厂；

	public abstract class AbstractFactory {
	    abstract void eatChickenGame();
	    abstract void racingCarGame();
	}

	public class TiMeiFactory extends AbstractFactory{
	    @Override
	    void eatChickenGame() {System.out.println("天美吃鸡游戏");}
	
	    @Override
	    void racingCarGame() {System.out.println("天美赛车游戏");}
	}
	
	public class GuangZiFactory extends AbstractFactory{
	    @Override
	    void eatChickenGame() {System.out.println("光子吃鸡游戏");}
	
	    @Override
	    void racingCarGame() {System.out.println("光子赛车游戏");}
	}

#### 建造者模式
特点：封装一个复杂对象的构建过程，并可以按步骤构造  
优点：易于扩展（例如新增一个具体建造者，只需要加入不同属性参数，无需修改原类）；产品的建造与表现分离解耦；创建过程细化易于控制清晰  
缺点：产品必有有共同点；内部复杂，会有很多建造类；  
情况：可以参考org.apache.http.client.utils.URIBuilder
	
	public class RequestBuilder {
	    private String scheme;
	    private String host;
	
	    public RequestBuilder(){}
	
	    public RequestBuilder setScheme(final String scheme){
	        this.scheme = scheme;
	        return this;
	    }
	
	    public RequestBuilder setHost(final String host){
	        this.host = host;
	        return this;
	    }
	
	    public String builder() {
	        return "RequestBuilder{" +
	                "scheme='" + scheme + '\'' +
	                ", host='" + host + '\'' +
	                '}';
	    }
	
	    public static void main(String[] args) {
	        String request = new RequestBuilder().setScheme ("http").setHost("www.google.com").builder ();
	        System.out.println (request);
	    }
	}

#### 原型模式
特点：通过复制现有的实例来创建新的实例
