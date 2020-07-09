## JDK8新特性与功能增强 - [官网](https://www.oracle.com/java/technologies/javase/8-whats-new.html)
> [教程](https://howtodoinjava.com/java-8-tutorial/)  

### 我个人选取部分适合我的点

### 1. Lambda Expressions
> [教程说明](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)  

	个人总结：
	使用lambda表达式，你需要实现一个功能接口，功能接口是仅包含一个抽象方法的接口（可以包含多个默认方法或静态方法）；
	使用lambda实现时，
		一： 可以省略该方法的名称
		二： 用()包含方法的参数，用逗号,分割参数列表，如只有一个可以省略
		三： 箭头标记 ->
		四： 方法的实现，即主体，如由单个表达式或语句直接写即可，java运行时候会评估是否有返回值，如是代码块即需要用{}
	JDK提供了标准功能接口，可在java.util.function包找到
	例子有用到：Predicate<T>、Consumer<T>、Function<T, R> 
	
	例子：
	class Person {
	    private Integer age;
	    private String name;
	
	    public Person(Integer age, String name) {
	        this.age = age;
	        this.name = name;
	    }
	
	    public Integer getAge() {
	        return age;
	    }
	
	    public void setAge(Integer age) {
	        this.age = age;
	    }
	
	    public String getName() {
	        return name;
	    }
	
	    public void setName(String name) {
	        this.name = name;
	    }
	
	    @Override
	    public String toString() {
	        return "Person{" +
	                "age=" + age +
	                ", name='" + name + '\'' +
	                '}';
	    }
	}
	public class Test {
		public static void printSimilarWith(
            	List<Person> personListlist,
	        	Predicate<Person> tester,
	            Function<Person, String> mapper,
	            Consumer<String> block) {
	        for (Person person : personListlist) {
	            if (tester.test(person)) {
	                String data = mapper.apply(person);
	                block.accept(data);
	            }
	        }
	    }

		public static void main(String[] args) {
			List<Person> list = new ArrayList<>();
	        list.add(new Person(19, "test"));
	        list.add(new Person(20, "zhangsan"));
	        list.add(new Person(21, "lisi"));
			Test.printSimilarWith(
                list,
                p -> p.getAge() == 20,
                p -> p.getName(),
                name -> System.out.println(name));
		}
	}

### 2. Colletions 的 java.util.stream包提供Stream API
> 
 
