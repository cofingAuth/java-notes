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
优点：在类初次化消耗资源多或需要繁琐的数据处理情况下，性能高  
缺点：需要注意浅拷贝的对象是否有引用对象的拷贝  
情况：类初次化消耗资源多或需要繁琐的数据处理，使用的对象属性信息基本一致  
实现：  
	- 实现Cloneable接口，重写clone方法  
	- 实现Serializable接口，重写clone方法  

	/**
	 * 浅拷贝：只拷贝按值传递的数据，比如String与基本类型；但引用对象、数组的拷贝是与原本的对象指向同一个内存地址
	 * 深拷贝：解决了浅拷贝的引用对象与数组拷贝指向同一个内存地址情况；
	 * @author Administrator
	 * @date 2020/5/25
	 */
	public class ShallowClone implements Cloneable{
	    public String name;
	    public String[] alias;
	
	    //浅拷贝
	    @Override
	    protected Object clone() throws CloneNotSupportedException {
	        return super.clone ();
	    }
	
	    /*深拷贝；则对数据/引用对象手动重新生成；
	    @Override
	    protected Object clone() throws CloneNotSupportedException {
	        ShallowClone sc = (ShallowClone) super.clone ();
	        sc.alias = Arrays.copyOf (sc.alias, alias.length);
	        return sc;
	    }*/
	}

	/**
	 * 深拷贝：序列化实现，首先把对象序列化则写进二进制流，然后再反序列化则从流中读出来
	 */
	public class DeepClone implements Serializable {
	    public String name;
	    public String[] alias;
	
	    @Override
	    protected Object clone() throws CloneNotSupportedException {
	        try {
	            ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream ();
	            ObjectOutputStream objectOutputStream = new ObjectOutputStream (byteArrayOutputStream);
	            objectOutputStream.writeObject (this);
	            ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream (byteArrayOutputStream.toByteArray ());
	            ObjectInputStream objectInputStream = new ObjectInputStream (byteArrayInputStream);
	            return objectInputStream.readObject ();
	        } catch (IOException e) {
	            e.printStackTrace ();
	        } catch (ClassNotFoundException e) {
	            e.printStackTrace ();
	        }
	        return null;
	    }
	}

#### 适配器模式
特点：将一个类的方法接口转换成客户希望的另外一个接口  
优点：可以让任何两个没有关联的类一起运行、提高类的复用性、增加类的透明度、灵活性好  
缺点：过多地使用适配器，会让系统非常零乱，不易整体进行把握  
实现：类的适配器模式（继承）、对象的适配器模式（引用对象）  
情况：服务订单操作场景
	
	/************************ 类的适配器模式（继承）************************/
	/**
	 * 订单服务提交接口
	 */
	public interface OrderServiceSave {
	    void save();
	}

	/**
	 * 把数据保存到数据库的操作类
	 */
	public class DoSaveOrderSerivce implements OrderServiceSave{
	    @Override
	    public void save() {
	        System.out.println ("save database");
	    }
	}

	/**
	 * 目标(Target)角色,这就是所期待得到的接口
	 */
	public interface SafetyHandle {
	    void safetySave();
		void safetyGet();
	}

	/**
	 * 适配器
	 */
	public class DatabaseSafetyHandle extends DoSaveOrderSerivce implements SafetyHandle{
	    @Override
	    public void safetySave() {
	        System.out.println ("安全1：记录操作者");
	        super.save ();
	        System.out.println ("安全2：具体...数据已保存成功");
	    }
		@Override
	    public void safetyGet() {}
	}
	
	/************************ 对象的适配器模式（引用对象）************************/
	/**
	 * 从数据库把数据读出来的操作类
	 */
	public class DoGetOrderSerivce implements OrderServiceGet{
	    @Override
	    public void get() {
	        System.out.println ("get order data from database");
	    }
	}
	
	/**
	 * 适配器
	 */
	public class DatabaseSafetyHandleImpl implements SafetyHandle{
	    private OrderServiceSave orderServiceSave;
	    private OrderServiceGet orderServiceGet;
	
	    public DatabaseSafetyHandleImpl(OrderServiceSave orderServiceSave, OrderServiceGet orderServiceGet){
	        this.orderServiceSave = orderServiceSave;
	        this.orderServiceGet = orderServiceGet;
	    }
	
	    @Override
	    public void safetySave() {
	        System.out.println ("安全1：记录操作者");
	        this.orderServiceSave.save ();
	        System.out.println ("安全2：具体...数据已保存成功");
	    }
	
	    @Override
	    public void safetyGet() {
	        System.out.println ("安全1：验证");
	        this.orderServiceGet.get ();
	    }
	}
	


#### 装饰者模式
特点：动态的给对象添加新的功能  
优点：1. 开闭原则; 2. 装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能; 3. 装饰类和被装饰类可以独立发展，不会相互耦合;   
缺点：1. 会出现更多的代码，更多的类，增加程序的复杂性; 2. 动态装饰时，多层装饰时会更复杂   
与适配器的不同：装饰者模式和被装饰的类要实现同一个接口，或者装饰类是被装饰的类的子类。 适配器模式和被适配的类具有不同的接口  
情况：服务订单操作场景，可以引入不同服务商进行操作

	/**
	 * 订单服务接口：为简单化，只定义了一个提交方法
	 */
	public interface OrderService {
	    void save();
	}
	
	/**
	 * 订单基础操作类
	 */
	public class OrderBase implements OrderService {
	    @Override
	    public void save() {
	        System.out.println ("操作保存到数据库");
	    }
	}

	/**
	 * 服务订单操作的装饰者抽象类
	 */
	public abstract class AbstractOrderDecorator implements OrderService{
	    private OrderService orderService;
	
	    public AbstractOrderDecorator(OrderService orderService){
	        this.orderService = orderService;
	    }
	
	    @Override
	    public void save() {
	        this.orderService.save ();
	    }
	}
	
	/**
	 * alibaba装饰者服务订单操作，需要先把订单发送到alibaba
	 */
	public class AlibabaOrder extends AbstractOrderDecorator{
	
	    public AlibabaOrder(OrderService orderService) {
	        super (orderService);
	    }
	
	    @Override
	    public void save() {
	        System.out.println ("订单先发送到alibaba");
	        super.save ();
	    }
	}

#### 代理模式
特点：为其他对象提供一个代理以便控制这个对象的访问  
优点：1、职责清晰。 2、高扩展性。 3、智能化;  
缺点：增加了客户端和真正主题之间中间层;  
实现：静态代理、动态代理（JDK、CGLIB）  
情况：火车票，黄牛相当于代理；我们可以通过黄牛买票，黄牛不仅能代售点卖票，还额外收取手续费，同时屏蔽了售点的其它功能（如退票）；

	public interface SubjectService {
	    void push();
	    void pull();
	}

	public class RealSubject implements SubjectService{
	    @Override
	    public void push() {
	        System.out.println ("push");
	    }
	
	    @Override
	    public void pull() {
	        System.out.println ("pull");
	    }
	}

	/**
	 * 静态代理
	 * 目标对象、代理对象需要实现同一接口；灵活度不够，如接口修改，目标、代理对象都需要维护
	 */
	public class SubjectProxy implements SubjectService{
	    private SubjectService subjectService;
	
	    public SubjectProxy(SubjectService subjectService) {
	        this.subjectService = subjectService;
	    }
	
	    @Override
	    public void push() {
	        System.out.println ("proxy start");
	        this.subjectService.push ();
	        System.out.println ("proxy end");
	    }
	
	    @Override
	    public void pull() {
	        throw new RuntimeException ("not support");
	    }
	}

	/**
	 * JDK动态代理
	 * 代理对象不需要实现接口，但是目标对象必须实现接口
	 */
	public class ProxyFactory {
	    private Object target;
	
	    public ProxyFactory(Object target) {
	        this.target = target;
	    }
	
	    public Object getProxyInstance(){
	        return Proxy.newProxyInstance (this.target.getClass ().getClassLoader (),
	                this.target.getClass ().getInterfaces (),
	                (proxy, method, args) -> {
	                    System.out.println ("do method start");
	                    return method.invoke (this.target, args);
	                });
	    }
	}

	/**
	 * CGLIB动态代理
	 * 目标类没有必须要求需要实现接口；CGLIB生成的代理类继承目标类；
	 */
	public class CGLibProxy implements MethodInterceptor {
	    public Object getProxyInstance(Object target){
	        //工具类
	        Enhancer enhancer = new Enhancer();
	
	        //设置父类
	        enhancer.setSuperclass (target.getClass ());
	
	        //设置回调
	        enhancer.setCallback (this);
	
	        //生成代理类对象
	        return enhancer.create ();
	    }
	
	    @Override
	    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
	        System.out.println ("start: " +  method.getName ());
	        Object object = methodProxy.invokeSuper (o, objects);
	        System.out.println ("end: " + method.getName ());
	        return object;
	    }
	
	    public static void main(String[] args) {
	        CGLibProxy proxy = new CGLibProxy();
	        RealSubject realProxy = (RealSubject)proxy.getProxyInstance (new RealSubject());
	        realProxy.push ();
	    }
	}

#### 外观模式
特点：对外提供一个统一的方法，来访问子系统中的一群接口  
优点：1、减少系统相互依赖。 2、提高灵活性。 3、提高了安全性  
缺点：不符合开闭原则  
意图：为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用  
简单来说：针对相同功能进行分组封装，简化客户端调用，屏蔽内部实现逻辑

	public interface SpiderService {
	    void start();
	}

	/**
	 * 外观模式
	 * 蜘蛛爬虫，为客户端调用提供一个接口，组合了一组接口处理；
	 */
	public class SpiderFacede implements SpiderService{
	    @Override
	    public void start() {
	        System.out.println ("引用对象1：蜘蛛抓取数据");
	        System.out.println ("引用对象2：解析数据");
	        System.out.println ("引用对象2：保存数据");
	    }
	}

#### 组合模式
特点：将对象组合成树形结构以表示“”部分-整体“”的层次结构  
优点：简化客户端代码，处理单个对象和组合对象；组合体新增不影响客户端使用；  
缺点：设计较复杂，客户端需要花更多时间理清类之间的层次关系；  
情况：配置文件处理类，java处理类，xml处理类

	public interface Configurer {
	    void addConfigurer(List<Configurer> configurers );
	    String getConfigurerName();
	    void matchHandler(String name);
	}
	
	public class JavaConfigurer implements Configurer{
	    @Override
	    public void addConfigurer(List<Configurer> configurers) {
	    }
	
	    @Override
	    public String getConfigurerName() {
	        return "java";
	    }
	
	    @Override
	    public void matchHandler(String name) {
	        System.out.println ("match success; do thing");
	    }
	}

	public class XmlConfigurer implements Configurer{
	    @Override
	    public void addConfigurer(List<Configurer> configurers) {
	    }
	
	    @Override
	    public String getConfigurerName() {
	        return "xml";
	    }
	
	    @Override
	    public void matchHandler(String name) {
	        System.out.println ("match success; do thing");
	    }
	}

	public class ConfigurerComposite implements Configurer{
	    private final List<Configurer> configurers = new ArrayList<> ();
	    @Override
	    public void addConfigurer(List<Configurer> configurers) {
	        this.configurers.addAll (configurers);
	    }
	
	    @Override
	    public String getConfigurerName() {
	        return "composite";
	    }
	
	    @Override
	    public void matchHandler(String name) {
	        for(Configurer configurer : this.configurers){
	            if(configurer.getConfigurerName ().equals (name)) {
	                configurer.matchHandler (name);
	                break;
	            }
	        }
	    }
	
	    public static void main(String[] args) {
	        Configurer java = new JavaConfigurer ();
	        Configurer xml = new XmlConfigurer ();
	        List<Configurer> configurerList = new ArrayList<> ();
	        configurerList.add (java);
	        configurerList.add(xml);
	        Configurer cc = new ConfigurerComposite();
	        cc.addConfigurer ( configurerList);
	        //客户端调用
	        cc.matchHandler ("java");
	        cc.matchHandler ("xml");
	    }
	}

#### 桥接模式
特点：将抽象部分和它的实现部分分离，使它们都可以独立的变化  
优点：分离抽象和实现部分; 更好的扩展性  
角色：抽象化角色、修正抽象化角色、实现化角色、具体实现化角色  
场景：电脑：品牌（联想、雷神）、类型（笔记本、台式），这两个维度都是需要相互作用构成一起  
更好例子：可以参考JDBC使用桥接模式（Driver、DriverManager、JDBC的API、具体驱动如mysql）

	/**
	 * 实现化角色
	 */
	public interface Brand {
	    void connect();
	}
	
	/**
	 * 具体实现化角色：雷神
	 */
	public class ThunderobotBrand implements Brand{
	    @Override
	    public void connect() {
	        System.out.println ("接入雷神驱动");
	    }
	}
	
	/**
	 * 具体实现化角色：联想
	 */
	public class LenovoBrand implements Brand{
	    @Override
	    public void connect() {
	        System.out.println ("接入联想驱动");
	    }
	}
	
	/**
	 * 抽象化角色：计算机
	 */
	public abstract class Computer {
	    protected Brand brand;
	
	    protected Computer(Brand brand) {
	        this.brand = brand;
	    }
	
	    public abstract void build();
	}
	
	/**
	 * 修正抽象化角色：台式电脑
	 */
	public class DesktopComputer extends Computer{
	    public DesktopComputer(Brand brand) {
	        super (brand);
	    }
	
	    @Override
	    public void build() {
	        System.out.println ("台式组装");
	        super.brand.connect ();
	    }
	}
	
	/**
	 * 修正抽象化角色：笔记本电脑
	 */
	public class NotebookComputer extends Computer{
	    public NotebookComputer(Brand brand) {
	        super (brand);
	    }
	
	    @Override
	    public void build() {
	        System.out.println ("笔记本组装");
	        super.brand.connect ();
	    }
	}

#### 享元模式
特点：通过共享技术来有效的支持大量细粒度的对象  
优点：相同对象只要保存一份，这降低了系统中对象的数量，从而降低了系统中细粒度对象给内存带来的压力  
缺点：提高了系统的复杂度，需要分离出外部状态和内部状态，而且外部状态具有固有化的性质，不应该随着内部状态的变化而变化，否则会造成系统的混乱  
看法：看起来像单例模式  
情况：武器，玩家可能拥有相同得武器

	/**
	 * 抽象享元角色
	 */
	public interface Weapon {
	    void display();
	}
	/**
	 * 具体享元：炮武器
	 */
	public class GunWeapon implements Weapon{
	    @Override
	    public void display() {
	        System.out.println ("炮武器");
	    }
	}
	
	/**
	 * 具体享元：剑武器
	 */
	public class SwordWeapon implements Weapon {
	    @Override
	    public void display() {
	        System.out.println ("剑武器");
	    }
	}
	/**
	 * 享元工厂
	 */
	public class WeaponFactory {
	    private static final Map<String, Weapon> weaponMap = new HashMap<> ();
	
	    public static Weapon getWeapon(String weaponName){
	        Weapon weapon = weaponMap.get (weaponName);
	        if(weapon == null){
	            try {
	                weapon = (Weapon) Class.forName (weaponName).newInstance ();
	            } catch (ClassNotFoundException | InstantiationException | IllegalAccessException e) {
	                e.printStackTrace ();
	                return null;
	            }
	            weaponMap.put (weaponName, weapon);
	        }
	        return weapon;
	    }
	}

#### 策略模式
特点：定义一系列算法，把他们封装起来，并且使它们可以相互替换  
优点：算法可以自由切换 避免使用多重条件判断 扩展性良好  
缺点：策略类会增多 所有策略类都需要对外暴露  
情况：实现同一个接口的不同的类，可以任意地替换；例如：生成订单前，需要做不同的判断，为了演示策略模式，我只判断一种情况  

	public interface OrderCheck {
	    void run () throws Exception;
	}

	/**
	 * 策略：验证是否身份
	 */
	public class ValidIdentidyOrderCheck implements OrderCheck{
	    @Override
	    public void run() throws Exception {
	        System.out.println ("判断是游客，还是有效的用户");
	    }
	}
	/**
	 * 策略：验证是否收到订单的金额
	 */
	public class ValidMoneyOrderCheck implements OrderCheck{
	    @Override
	    public void run() throws Exception {
	        System.out.println ("验证是否收到对应金额");
	    }
	}

	public class UseStrategy {
	    private OrderCheck orderCheck;
	
	    public void setOrderCheck(OrderCheck orderCheck) {
	        this.orderCheck = orderCheck;
	    }
	
	    public static void main(String[] args) {
	        UseStrategy useStrategy = new UseStrategy();
	        //这订单需要验证身份
	        useStrategy.setOrderCheck (new ValidIdentidyOrderCheck ());
	        //这订单需要验证金额
	        useStrategy.setOrderCheck (new ValidMoneyOrderCheck ());
	    }
	}

#### 模板方法模式
特点：定义一个算法结构，而将一些步骤延迟到子类实现  
优点：封装不变，扩展可变；流程由父类控制，子类进行实现  
缺点：每一个不同的实现都需要一个子类来实现，导致类的个数增加，使得系统更加庞大  
情况：订单流程，需要验证、保存数据库、发送短信；  
角色：  
	1. 抽象模板：抽象模板类，定义了一套算法框架/流程；  
	2. 具体实现：具体实现类，对算法框架/流程的某些步骤进行了实现；  

	/**
	 * 抽象模板
	 * 在这个例子情况下，抽象验证方法，可以优化成配合工厂方法+数据库配置动态改动和复重验证类
	 */
	public abstract class AbstractTemplate {
	    public final void run(Object object){
	        valid(object);
	        save ();
	        sendMessage ();
	    }
	
	    public abstract void valid(Object object);
	
	    private void save(){
	        System.out.println ("save database");
	    }
	
	    public void sendMessage(){
	        System.out.println ("选择性覆盖的方法，发短信");
	    }
	}
	
	/**
	 * 具体实现案例一
	 */
	public class CommonTemplate extends AbstractTemplate{
	    @Override
	    public void valid(Object object) {
	        System.out.println ("valid 1");
	        System.out.println ("valid 2");
	        System.out.println ("valid 3");
	    }
	}
	
	/**
	 * 具体实现案例二
	 */
	public class CommonSendMsgTemplate extends AbstractTemplate{
	    @Override
	    public void valid(Object object) {
	        System.out.println ("valid 1");
	        System.out.println ("valid 2");
	    }
	
	    @Override
	    public void sendMessage() {
	        System.out.println ("实现发送短信");
	    }
	}

#### 观察者模式
特点：对象间的一对多的依赖关系；又被称为"发布-订阅"模式  
优点：观察者和被观察者是抽象耦合的 建立一套触发机制  
缺点：  
	1. 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间  
	2. 如果观察者和观察目标间有循环依赖，可能导致系统崩溃  
	3. 没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的  
角色：抽象被观察者角色、抽象观察者角色、具体被观察者角色、具体观察者角色  
场景：一个公众号，公众号推送出消息，那么关注它的人都能收到推送的消息  
说明：JDK内置有观察者（Observer接口需要实现）、被观察者（Observable类需要使用继承）

	/**
	 * 抽象被观察者角色
	 */
	public interface Observerable {
	    void registerObserver(Observer observer);
	    void removeObserver(Observer observer);
	    void notifyObserver(String message);
	}

	/**
	 * 抽象观察者角色
	 * 定义一个update()方法，当被观察者调用notifyObserver()方法时，观察者的update()会被调用
	 */
	public interface Observer {
	    void update(String message);
	}

	/**
	 * 被观察者
	 */
	public class WechatServer implements Observerable{
	    private List<Observer> observerList = new ArrayList<> ();
	
	    @Override
	    public void registerObserver(Observer observer) {
	        this.observerList.add (observer);
	    }
	
	    @Override
	    public void removeObserver(Observer observer) {
	        this.observerList.remove (observer);
	    }
	
	    @Override
	    public void notifyObserver(String message) {
	        for(Observer observer : this.observerList) {
	            observer.update (message);
	        }
	    }
	}

	/**
	 * 观察者
	 */
	public class FollowObserver implements Observer{
	    @Override
	    public void update(String message) {
	        System.out.println ("observer receives message : " + message);
	    }
	}

#### 责任链模式
特点：将请求的发送者和接收者解耦，使的多个对象都有处理这个请求的机会  
优点：1、降低耦合度。它将请求的发送者和接收者解耦。 2、简化了对象。使得对象不需要知道链的结构。 3、增强给对象指派职责的灵活性。通过改变链内的成员或者调动它们的次序，允许动态地新增或者删除责任。 4、增加新的请求处理类很方便。  
缺点：1、不能保证请求一定被接收。 2、系统性能将受到一定影响，而且在进行代码调试时不太方便，可能会造成循环调用。 3、可能不容易观察运行时的特征，有碍于除错。  
场景：积分商城，进行兑换商品，商品可能会有需要积分的，有可能免费兑换等情况；如配合数据库配置，他可以很灵活的控制兑换流程；

	//请求体
	public class ChainRequest {
	    /**
	     * 简单用这个表示类型：0表示需要积分，1不需要积分
	     */
	    private Integer type;
	    public Integer getType() {
	        return type;
	    }
	
	    public void setType(Integer type) {
	        this.type = type;
	    }
	}
	
	//责任链的抽象处理类
	public abstract class AbstractHandler {
	    protected AbstractHandler nextHandler;
	
	    public void setNextHandler(AbstractHandler nextHandler) {
	        this.nextHandler = nextHandler;
	    }
	
	    protected abstract void handler(ChainRequest chainRequest);
	}
	
	//处理需要积分的处理类
	public class IntegralCheck extends AbstractHandler{
	    @Override
	    protected void handler(ChainRequest chainRequest) {
	        if(chainRequest.getType () == 0){
	            System.out.println ("处理积分商品");
	            return;
	        }
	
	        if(super.nextHandler != null){
	            super.nextHandler.handler (chainRequest);
	        }
	    }
	}
	
	//处理不需要积分的处理类
	public class UnIntegralCheck extends AbstractHandler{
	    @Override
	    protected void handler(ChainRequest chainRequest) {
	        if(chainRequest.getType () == 1){
	            System.out.println ("处理不需要积分商品");
	            return;
	        }
	
	        if(super.nextHandler != null){
	            super.nextHandler.handler (chainRequest);
	        }
	    }
	}
	
	//测试
	public class Test {
	    public static void main(String[] args) {
	        AbstractHandler integralCheck = new IntegralCheck ();
	        AbstractHandler unIntegralCheck = new UnIntegralCheck ();
	
	        integralCheck.setNextHandler (unIntegralCheck);
	
	        ChainRequest chainRequest = new ChainRequest ();
	        chainRequest.setType (1);
	
	        integralCheck.handler (chainRequest);
	    }
	}

#### 命令模式
特点：将命令请求封装为一个对象，使得可以用不同的请求来进行参数化  
优点：1、降低了系统耦合度。 2、新的命令可以很容易添加到系统中去  
缺点：使用命令模式可能会导致某些系统有过多的具体命令类  
场景：声音控制门的开关

	public interface Command {
	    void execute();
	}
	
	/**
	 * received 真正的命令执行对象
	 */
	public class Controller {
	    public void open(){
	        System.out.println ("open the door");
	    }
	
	    public void close(){
	        System.out.println ("close the door");
	    }
	}
	
	/**
	 * Command
	 */
	public class OpenCommand implements Command {
	    private Controller controller;
	
	    public OpenCommand(Controller controller) {
	        this.controller = controller;
	    }
	
	    @Override
	    public void execute() {
	        this.controller.open ();
	    }
	}
	
	/**
	 * Command
	 */
	public class CloseCommand implements Command {
	    private Controller controller;
	
	    public CloseCommand(Controller controller) {
	        this.controller = controller;
	    }
	
	    @Override
	    public void execute() {
	        this.controller.close ();
	    }
	}
	
	public class Test {
	    public static void main(String[] args) {
	        Controller controller = new Controller ();
	
	        Command openCommand = new OpenCommand (controller);
	        Command closeCommand = new CloseCommand (controller);
	
	        //Invoker调用者
	        openCommand.execute ();
	        closeCommand.execute ();
	    }
	}


