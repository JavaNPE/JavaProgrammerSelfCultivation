# Java8Lambda表达式看这一篇就够了

有一天，一位新来的同事跑过来问茶哥，“代码里面一个横杠+一个箭头是什么意思呀”。这熟悉的场景也曾发生在茶哥身上，想当年茶哥刚进入职场工作的时候，遇到那些奇奇怪怪的字符，茶哥也是一脸懵逼，而且还不好意思问别人，生怕问出来怕别人嘲笑的那种尴尬。

![查看源图像](https://hediancha-1312143060.cos.ap-shanghai.myqcloud.com/202091718455592687.jpg)

依稀记得茶哥第一次做项目的时候，写代码的过程中报了一个类型转换异常ClassCastException的报错信息，年少不知Java8的好，错把List当成宝，毕竟当年比较菜，薪水只有区区的六千块，这种类型转换的报错被我捣鼓了半天硬生生的还没搞出来。然后找了傍边一大哥，大哥二话没说，写了一行我不太理解的代码放在了那，我重新运行了一下程序，竟然“奇迹”般的跑起来了。赶紧Copy到我的Typora中好好研究研究。

自此茶哥，开始下定决心好好学习Java8。故事分享完了，那么今天就以这个类型转换为切入点，我们好好谈一谈Java8所谓的新特新。

作为新手小白，那么如果项目中需要将List转成Map你会怎么做？茶哥，这个我知道呀，用for循环呀，然后再给它put进去，岂不美哉。额，果然年少不知Java8的好呀！其实用循环的方式对List类型和Map类型进行互转是我们作为新手程序员都可以想到了。但是，如果你在开发过程中使用这种map.put()方式绝对会被鄙视的，直接代码审核不通过，打回去重新Coding。

本例作为演示先定义一个List集合，然后将4位面壁者的个人信息存放到我们的List集合中，本例茶哥稍微偷懒一下，只将放了罗辑的个人信息，然后开始执行我们的“破壁计划”。

```java
        List<UserInfoDto> userInfoDtos = new ArrayList<>();

        UserInfoDto userInfoDto = new UserInfoDto();
        userInfoDto.setUserId("Id1008791");
        userInfoDto.setPhone("17108891984");
        userInfoDto.setUserName("罗辑");

        userInfoDtos.add(userInfoDto);
```

如果将以上执剑人的List集合转成Map类型的话你会怎么做呢？反正茶哥当年也是使用的iter的方式循环遍历然后在put操作，在当时我个人觉得，也算是一顿操作猛如虎吧，但是如果你这么写出来，只能说明你还仅仅是初悟棋道，不懂棋理，绝对会鄙视的。

```java
        Map<String, UserInfoDto> maps = new HashMap<>();
        for (UserInfoDto userInfo : userInfoDtos) {
            maps.put(userInfo.getUserId(), userInfo);
        }
```

![image-20220528101357986](https://hediancha-1312143060.cos.ap-shanghai.myqcloud.com/image-20220528101357986.png)

那么我们对比一下使用Java8的将List转成Map的方式，又是一通操作，如果对Java8还不是很熟悉的话，看着这样的是不是特别的不舒服呀，不禁心中来了一句mmp，这种写法简直就是反人类啊！

```java
Map<String, UserInfoDto> userInfoDtoMap = userInfoDtos.stream().collect(Collectors.toMap(UserInfoDto::getUserId, Function.identity(), (a, b) -> b));
```

![image-20220528103909754](https://hediancha-1312143060.cos.ap-shanghai.myqcloud.com/image-20220528103909754.png)

不过你要是学会了Java8的新特新，相信你一定会爱不释手，然后惊呼，卧槽还是Java8牛🍺。毕竟目前企业级别主流的jdk1.8的版本。

# 为什么使用 Lambda 表达式？



在回答这个问题之前，茶哥想问一下看这篇文章的读者，你们希望实名还是匿名。实名制相当于把你的底裤都扒拉开，没有一点隐私的权力，在老大哥的关注下，发表一些相反的言论，都有被请去喝茶的可能性，所以说匿名还是有匿名的好处的。

<img src="https://hediancha-1312143060.cos.ap-shanghai.myqcloud.com/image-20220528132049502.png" alt="image-20220528132049502" style="zoom:50%;" />

Lambda 就是一个匿名函数，既然都匿名了那就比较随意了，比较灵活了，我们可以把 Lambda 表达式理解为是一段可以传递的代码，将代码像数据一样进行传递，在暗中就把活给干了，是不是非常的nice。所以以前用匿名实现类表示的现在都可以用Lambda表达式来写。使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了进一步的提升。



# **Lambda表达式**格式详解

1. Java8中引入了一个新的操作符“->”，称为箭头运算符，或者Lambda运算符

2. 作用：就是用于分隔前后两部分的。

3. 左边：表示Lambda表达式的参数列表（接口中，定义的抽象方法的参数）其实就是接口中的抽象方法的形参列表

4. 右边：表示的是方法的方法体，Lambda体，其实就是重写的抽象方法的方法体

5. 语法格式1：没有参数，也没有返回值

   1. `() -> System.out.println("点一杯绿茶");`

6. 语法格式2：有一个参数，没有返回值

   1. `(x) -> System.out.println(x * x);`
   2.  说明：如果只有一个参数，那么小括号可以省略

7. 语法格式3：有多个参数，没有返回值，格式和语法格式2相同

   1. `(x, y) -> System.out.println(x + y);`

8. 语法格式4：接口中需要重写的方法，方法内容有多句，需要给多句话加上大括号

   1. 例如：`(x, y) -> {int result = x + y; return result;}`

   2. 注意事项：如果Lambda体中语句只有一句，那么大括号可以省略不写；如果大括号中只有一条语句，并且是return语句，那么return关键字也可以省略不写（如果要省略return关键字，就必须省略大括号）

       例如：`(x, y) -> x + y`

## 语法格式一：Lambda 无参，无返回值

格式我们算是介绍完毕了，那么就动手实践一下，开启一个线程并在控制台打印‘’**点一杯绿茶**“，传统的写法我们可以这样写，创建Runnable()线程然后实现它的run()方法。

```java
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            System.out.println("点一杯绿茶");
        }
    };
    r1.run();
```

如果我们使用Lambda表达式的写法，仅需一行代码就可以搞定了，可以说是非常的简洁吧。

```java
    Runnable r2 = () -> System.out.println("点一杯绿茶");
    r2.run();
```

传统写法比较两个数的大小，我们可以这么写，可以看到又是new又是重写的，相对的繁琐。

```java
    Comparator<Integer> com1 = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return Integer.compare(o1,o2);
        }
    };

    int compare1 = com1.compare(18,23);
    System.out.println(compare1);
```

使用Lambda表达式我们可以写成这样的形式，是不是一下子就简洁了很多，对比传统给写法，我们可以看到”->“的左边也就是参数部分，定义了两个变量，然后直接通过Integer.compare(o1,o2)的形式直接计算并完成return Integer.compare(o1,o2);这部分操作。

```java
    Comparator<Integer> com2 = (o1,o2) -> Integer.compare(o1,o2);

    int compare2 = com2.compare(23,18);
    System.out.println(compare2);
```

那么还有没有更简洁的写法呢？答案是：当然有，直接以这种的形式`Integer :: compare;`同样可以实现相同的功能，又可以减少代码量了。

```java
    Comparator<Integer> com3 = Integer :: compare;

    int compare3 = com3.compare(18,23);
    System.out.println(compare3);
```

好了关子暂时先卖到这，以上呢Java8的砖算是抛出来了，那么接下来就跟着茶哥好好把Java8的Lambda表达式研究研究一下。

## 语法格式二：Lambda 需要一个参数，但是没有返回值

```java
    // 传统写法
	Consumer<String> con = new Consumer<String>() {
        @Override
        public void accept(String s) {
            System.out.println(s);
        }
    };
    con.accept("赫点茶");

    System.out.println("---------------------------------");

	// Lambda表达式写法
    Consumer<String> con1 = (String s) -> {
        System.out.println(s);
    };
    con1.accept("赫点茶");
```

## 法格式三：数据类型可以省略，因为可由编译器推断得出，称为“类型推断”



```java
    // 传统写法
	Consumer<String> con1 = (String s) -> {
        System.out.println(s);
    };
    con1.accept("赫点茶");

    System.out.println("---------------------------------");

	// Lambda表达式写法
    Consumer<String> con2 = (s) -> {
        System.out.println(s);
    };
    con2.accept("赫点茶");
```

## 语法格式四：Lambda 若只需要一个参数时，参数的小括号可以省略

```java
    // 传统写法
	Consumer<String> con1 = (s) -> {
        System.out.println(s);
    };
    con1.accept("赫点茶");

    System.out.println("---------------------------------");

	// Lambda表达式写法1
    Consumer<String> con2 = s -> {
        System.out.println(s);
    };
    con2.accept("赫点茶");

	// Lambda表达式写法2
	Consumer<String> con2 = s -> System.out.println(s);
```

## 语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值

```JAVA
		// 传统写法
		Comparator<Integer> com1 = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                System.out.println(o1);
                System.out.println(o2);
                return o1.compareTo(o2);
            }
        };

        System.out.println(com1.compare(12,21));

    	System.out.println("---------------------------------");

		// Lambda表达式写法
        Comparator<Integer> com2 = (o1,o2) -> {
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        };

        System.out.println(com2.compare(12,6));

```



## 语法格式六：当 Lambda 体只有一条语句时，return 与大括号若有，都可以省略

```java
    // 传统写法
	Comparator<Integer> com1 = (o1,o2) -> {
        return o1.compareTo(o2);
    };

    System.out.println(com1.compare(12,6));

    System.out.println("---------------------------------");

	// Lambda表达式写法
    Comparator<Integer> com2 = (o1,o2) -> o1.compareTo(o2);
    System.out.println(com2.compare(12,21));
```















