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
> 在流上，可以执行一个或多个操作; 流操作是中间操作或者终端操作；中间操作返回流本身，终端操作返回某种类型的结果  
> 然而最后我并不知道写什么好，感觉想使用就看源码的注释吧，有例子也会说明；那我就随便放几个用法吧  
> 主要流上处理不影响源，然而如果需要修改，可以在流上修改并生成新的对象即可

	public class Test {
	    public static void main(String[] args) {
	        List<Integer> list = Arrays.asList(1, 2, 3, 4, 1, 4);
	        //流，先过滤，只留下元素>50,然后生成一个新的列表，最后遍历
	        List<Integer> collect = list.stream().filter(e -> e > 50).collect(Collectors.toList());
	        collect.stream().forEach(e -> System.out.println(e));
	
	        //流，列表大小
	        System.out.println(list.stream().count());
	
	        //流，先降序，再遍历
	        list.stream().sorted((t1, t2) -> t2 - t1).forEach(e -> System.out.println(e));
	
	        //流，先元素都加一，再遍历
	        list.stream().map(e -> e + 1).forEach(e -> System.out.println(e));
	
	        //流，1与元素的累加，这个可以看源码注释次方法的等效方法既懂
	        System.out.println(list.stream().reduce(1, (a, b) -> a + b));
	    }
	}

### 3. Base64 encoding and decoding
	以前Base64都是使用第三方包，现应该都使用JDK只带的
	Base64是网络上最常见的用于传输8Bit字节码的编码方式之一
	Base64是基于64个可打印的字符来表示二进制数据的方法  
	特点：
		1. 可以将任意的二进制数据进行Base64编码
		2. 数据加密之后，数据量会变大，变大1/3左右
		3. 编码后有个非常显著的特点，末尾有个=号
		4. 可进行反向解密
		5. Base64编码具有不可读性
	看了源码，你发现有编码解码有三种使用
		1. 我这里只说明下编码器，而解码器是互相对应的；
		2. 分别是: getEncoder()、getUrlEncoder()、getMimeEncoder()
		3. 其实源码注解也说的很清楚，码表分为table1[A-z0-9+/]与table2[A-z0-9-_]
		4. encoder : 是最基本的，使用table1，编码不会增加换行符
		5. urlEncoder : 是用于URL和文件名，使用table2，编码不会增加换行符
		6. mimeEncoder : 是使用table1，编码输出必须每行不超过76个字符，超过则在末尾追加\r\n作为换行，解码时由于表1找不到换行的符号，所以换行符号会被忽略
	例子：
	public class Test {
	    public static void main(String[] args) {
	        String content = "username:password";
	        Base64.Encoder encoder = Base64.getEncoder();
	        String encodedContent = encoder.encodeToString(content.getBytes(StandardCharsets.UTF_8));
	        System.out.println(encodedContent);
	
	        Base64.Decoder decoder = Base64.getDecoder();
	        byte[] decodedByteArray = decoder.decode(encodedContent);
	        System.out.println(new String(decodedByteArray));
	    }
	}

### 4. Join String Array – Convert Array to String
> String.join() 、 StringJoiner 与 lambda上Collectors.joining()  
> 这个看下代码即可，后两个都可以有 分隔符、前缀、后缀  

	public static void main(String[] args) {
        String joinDate = String.join("-", "2020", "07", "10");
        System.out.println(joinDate);//2020-07-10

        String joinListDate = String.join("-", Arrays.asList("2020", "07", "10"));
        System.out.println(joinListDate);//2020-07-10

        StringJoiner joiner = new StringJoiner(" ", "[", "]");
        joiner.add("who").add("are").add("you");
        System.out.println(joiner.toString());//[who are you]

        String collect = Arrays.asList("1024", "2048", "1").stream().collect(Collectors.joining(", "));
        System.out.println(collect);//1024, 2048, 1
    }

### 5. Functional Interfaces
> 功能接口，在lambda有提出功能接口;这接口称为单一抽象方法接口（SAM接口），只有一个抽象方法  
> 使用：@FunctionalInterface注解，编译器会提示你是否复合是功能接口，如不符合会提示错误

### 6. Networking
> 网络上相关的可以都看下:java.net.HttpURLConnection、java.net.URLPermission 

### 7. date time 
> 为ISO系统设置的，如2020-07-10T17:14:56.448

 
