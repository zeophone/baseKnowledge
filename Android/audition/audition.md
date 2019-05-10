#上海
## 电话面试
#### 问
1. 自我介绍
2. 你们公司人员配置，为什么安卓端要那么多人,你负责的模块，产品复杂吗？
3. 产品架构怎么处理的？你们的产品有大量重复的模块，如何处理的？还是使用传统的MVC吗？当初设计没有考虑到后期的扩展吗？
4. 为什么不去北京，北京IT不是发展更好吗？
4. 有了解过Kotlin吗？有学习过吗，怎么看待这个新语言？java中的null和Kotlin对此问题的解决。
4. 挑一个你github的项目介绍一下。有运用到什么数据结构吗？
4. 用过哪些第三方框架？介绍一下butterknife？butterknife注解怎么实现的有了解吗？
4. 主动介绍EventBus源码解析。
4. 介绍一下Rxjava和Retrofit，两者什么关系?
4. 为什么要用Retrofit，直接用OKhttp不好吗？
5. 有了解RxBus吗？和EventBus有什么区别？为什么不试一试RxBus呢？
4. 了解反射吗？你说反射可以修改方法，怎么修改？你说双重锁通过反射可以修改，不能保证单个实例，自己有测试过吗？没测试怎么保证可行？
4. 介绍一下四大组件
4. 进程和线程的区别
4. APP之间怎么通信？
4. APP开发中什么时候用到多进程？
4. 我问:技术部门的构成。
4. 我问:安卓组技术选型谁定的？开发人员是否可以对产品设计提建议？
4. 我问:如何合理架构产品保证后期的维护与迭代？

#### 答
1. ***。
2. ***。
3. 我们首先建造了一个和业务无关系的lib库，包括各种常用工具类，自定义控件；各种基类，比如BaseActivity,将各种重复代码放到了该base类中，比如EventBus、TAG的获取、自定义Activity栈等。至于EvenBus是否注册以抽象类的方式延迟到子类去实现，运用了模板模式实现。产品中确实存在很多重复的模块，比如选择图片的页面、录音页面等，这些明显重复的页面采用帧布局的方式布局，其它地方引入即可。当时有考虑采用MVP模式进行架构的，但是最后讨论没有达成一致的意见，原因是不太熟悉，怕影响工期。当然，这造成的后果是产品采用传统的方式耦合在一起，后期扩展就比较麻烦。
4. ***。因此就计划在上海工作。
5. Kotlin 是一个基于 JVM 的新的编程语言，由 JetBrains 开发。在2017 google IO大会上Kotlin已正式成为Android官方开发语言。
	1. 简洁、优雅----类型推导、不用写分号以及天然支持Lambda表达式、字符串模板
	2. 安全----不用进行非空判断以及类型转换判断等
	3. 函数、属性扩展----Kotlin提供了一种方法——可以在既不需要继承父类，也不需要使用类似装饰器设计模式的情况下，对类进行扩展。简直是黑科技！
	4. 兼容Java----可以相互转化，相互使用。
	5. 工具完善---Kotlin插件提供了一个Java->Kotlin的转换功能。
1. 介绍CSDN项目，数据库的优化，爬虫的优化等。
2. EventBus、glide、butterknife等。介绍一下butterknife如下，butterknife注解原理：可能很多人都觉得ButterKnife在bind(this)方法执行的时候通过反射获取ExampleActivity中所有的带有@Bind注解的属性并且获得注解中的R.id.xxx值，最后还是通过反射拿到Activity.findViewById()方法获取View，并赋值给ExampleActivity中的某个属性这是一个注解库的实现方式，比较原始，一个很大的缺点就是在Activity运行时大量使用反射会影响App的运行性能，造成卡顿以及生成很多临时Java对象更容易触发GC。ButterKnife显然没有使用这种方式，它用了Java AnnotationProcessing技术，就是在Java代码编译成Java字节码的时候就已经处理了@Bind、@OnClick（ButterKnife还支持很多其他的注解）这些注解了。@[http://blog.csdn.net/zcxwww/article/details/52205832](http://blog.csdn.net/zcxwww/article/details/52205832 "http://blog.csdn.net/zcxwww/article/details/52205832")
	1. 强大的View绑定和Click事件处理功能，简化代码，提升开发效率
	2. 方便的处理Adapter里的ViewHolder绑定问题
	3. 运行时不会影响APP效率，使用配置方便
	4. 代码清晰，可读性强
1. 首先采用双重锁方式获取EventBus单例，然后去执行注册过程。注册过程的第一步就是去注册的类中遍历所有方法找到所有onEvent(Object obj)方法（也就是@Subscriber)方法，将方法存到一个List中，包括该方法的各种信息：EventType(onEvent(Object obj)的obj)、线程模式、方法名等。第二步：根据EventType将onEventBus进行分类，最终存到一个Map中，其中k:EventType,V:包含onEvent所属的类、方法名、线程模式等信息。 而post会根据实参去map查找进行反射调用。
2. 介绍一下Rxjava和Retrofit，两者什么关系?
3. retrofit非常适合于restful url格式的请求，更多使用注解的方式提供功能。此外，由于其内部提供了ConverterFactory用于对返回的requestBody进行转化和特殊的requestBody的构造。
4. RxBus基于Rxjava,代码实现非常少。由于RxJava已经解决了订阅和线程切换问题，RxBus尚未解决的，也是重点需要解决的是事件分发的效率问题。RxJava天生就是类似sub/pub的观察者模式，而且很容易处理线程切换。而EventBus做的最重要的两件事就是事件订阅和线程控制。所以才有了RxBus区区30行代码就敢挑战EventBus的老大地位。测试中可以明显发现RxBus效率随订阅者增多而成比例下降。【不建议使用】
5. 反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。如下即实现双重锁被反射攻击造成多实例：

		/**
		     * 单例模式被java反射攻击
		     * @throws IllegalArgumentException
		     * @throws InstantiationException
		     * @throws IllegalAccessException
		     * @throws InvocationTargetException
		     * @throws SecurityException
		     * @throws NoSuchMethodException
		     */
		    
		    public static void attack() throws IllegalArgumentException, InstantiationException, IllegalAccessException, InvocationTargetException, SecurityException, NoSuchMethodException
		    {
		        Class<?> classType = Singleton.class;
		        Constructor<?> constructor = classType.getDeclaredConstructor(null);
		        constructor.setAccessible(true);
		        Singleton singleton = (Singleton) constructor.newInstance();
		        Singleton singleton2 = Singleton.newInstance();
		        System.out.println(singleton == singleton2);  //false
		    }

6. 四大组件
4. 进程和线程的区别
4. APP之间怎么通信？
4. APP开发中什么时候用到多进程？
4. 我问:技术部门的构成。
4. 我问:安卓组技术选型谁定的？开发人员是否可以对产品设计提建议？
4. 我问:如何合理架构产品保证后期的维护与迭代？


## 做项目

#### 问
* 要求做一个小项目（完成即可，不用去公司），题目如下
![](http://i.imgur.com/qlGJchP.png)

#### 答
* 这个最好的实现方式应该是自定义一个ViewGroup,但是当时没实现。而是采用了xml的形式实现一个，然后在主页面inclue即可。

* 实现效果如下

![](http://i.imgur.com/bVxmla1.jpg)
![](http://i.imgur.com/kknaoOw.jpg)
![](http://i.imgur.com/YLZ9EYV.jpg)

* 大致代码如下

		<?xml version="1.0" encoding="utf-8"?>
		<LinearLayout
		    xmlns:android="http://schemas.android.com/apk/res/android"
		    android:layout_width="300dp"
		    android:layout_height="50dp"
		    android:orientation="horizontal"
		    android:background="@drawable/bg_rectangle_gray"
		    android:gravity="center_vertical"
		    android:id="@+id/cv_ll_container"
		  >
		<CheckBox
		    android:id="@+id/cv_rb_click"
		    android:layout_width="20dp"
		    android:layout_height="20dp"
		    android:background="@drawable/circle_gray"
		    android:layout_marginLeft="16dp"
		    android:button="@null"
		    />
		
		    <TextView
		        android:id="@+id/cv_tv_txt"
		        android:layout_width="wrap_content"
		        android:layout_height="wrap_content"
		        android:layout_marginLeft="16dp"
		        android:text="首单立减"
		        android:textColor="@color/colorWhite"
		        android:textSize="16dp"
		        android:maxEms="10"/>
		</LinearLayout>


	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:id="@+id/activity_main"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:background="#e7e6e6"
	    android:orientation="vertical"
	    android:paddingBottom="@dimen/activity_vertical_margin"
	    android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    tools:context="com.gaoyuan.customerview4zaihui.MainActivity">
	
	    <include
	        android:id="@+id/ma_i_view1"
	        layout="@layout/customer_view"/>
	
	    <View
	        android:layout_width="match_parent"
	        android:layout_height="32dp"/>
	
	    <include
	        android:id="@+id/ma_i_view2"
	        layout="@layout/customer_view"/>
	
	    <View
	        android:layout_width="match_parent"
	        android:layout_height="32dp"/>
	
	    <include
	        android:id="@+id/ma_i_view3"
	        layout="@layout/customer_view"/>
	</LinearLayout>



		package com.gaoyuan.customerview4zaihui;
		
		import android.os.Bundle;
		import android.support.v7.app.AppCompatActivity;
		import android.widget.CheckBox;
		import android.widget.CompoundButton;
		import android.widget.LinearLayout;
		import android.widget.TextView;
		
		public class MainActivity extends AppCompatActivity {
		
		    //    第1个控件
		    private LinearLayout mCvLlContainer1;
		    private CheckBox mCvRbClick1;
		    private TextView mCvTvTxt1;
		    //      第2个控件
		    private LinearLayout mCvLlContainer2;
		    private CheckBox mCvRbClick2;
		    private TextView mCvTvTxt2;
		    //       第3个控件
		    private LinearLayout mCvLlContainer3;
		    private CheckBox mCvRbClick3;
		    private TextView mCvTvTxt3;
		
		    //inclue
		    private LinearLayout ma_i_view1;
		    private LinearLayout ma_i_view2;
		    private LinearLayout ma_i_view3;
		
		
		    @Override
		    protected void onCreate(Bundle savedInstanceState) {
		        super.onCreate(savedInstanceState);
		        setContentView(R.layout.activity_main);
		        initview();
		        initListener();
		    }
		
		    private void initListener() {
		        mCvRbClick1.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
		            @Override
		            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
		                if (isChecked) {
		                    mCvRbClick1.setBackgroundDrawable(getResources().getDrawable(R.drawable.cat_checked));
		                    mCvLlContainer1.setBackgroundDrawable(getResources().getDrawable(R.drawable.bg_rectangle_pink));
		                } else {
		                    mCvRbClick1.setBackgroundDrawable(getResources().getDrawable(R.drawable.circle_gray));
		                    mCvLlContainer1.setBackgroundDrawable(getResources().getDrawable(R.drawable.bg_rectangle_gray));
		                }
		            }
		        });
		        mCvRbClick2.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
		            @Override
		            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
		                if (isChecked) {
		                    mCvRbClick2.setBackgroundDrawable(getResources().getDrawable(R.drawable.cat_checked));
		                    mCvLlContainer2.setBackgroundDrawable(getResources().getDrawable(R.drawable.bg_rectangle_bule));
		                } else {
		                    mCvRbClick2.setBackgroundDrawable(getResources().getDrawable(R.drawable.circle_gray));
		                    mCvLlContainer2.setBackgroundDrawable(getResources().getDrawable(R.drawable.bg_rectangle_gray));
		                }
		            }
		        });
		        mCvRbClick3.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
		            @Override
		            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
		                if (isChecked) {
		                    mCvRbClick3.setBackgroundDrawable(getResources().getDrawable(R.drawable.cat_checked));
		                    mCvLlContainer3.setBackgroundDrawable(getResources().getDrawable(R.drawable.bg_rectangle_green));
		                } else {
		                    mCvRbClick3.setBackgroundDrawable(getResources().getDrawable(R.drawable.circle_gray));
		                    mCvLlContainer3.setBackgroundDrawable(getResources().getDrawable(R.drawable.bg_rectangle_gray));
		                }
		            }
		        });
		    }
		
		    private void initview() {
		        //inclue
		        ma_i_view1 = (LinearLayout) findViewById(R.id.ma_i_view1);
		        ma_i_view2 = (LinearLayout) findViewById(R.id.ma_i_view2);
		        ma_i_view3 = (LinearLayout) findViewById(R.id.ma_i_view3);
		
		
		        //第一个
		        //mCvLlContainer1 = (LinearLayout) ma_i_view1.findViewById(R.id.cv_ll_container);
		        mCvLlContainer1 = ma_i_view1;
		        mCvRbClick1 = (CheckBox) ma_i_view1.findViewById(R.id.cv_rb_click);
		        mCvTvTxt1 = (TextView) ma_i_view1.findViewById(R.id.cv_tv_txt);
		        mCvTvTxt1.setText("首单立减");
		        //第二个
		        //mCvLlContainer2 = (LinearLayout) ma_i_view2.findViewById(R.id.cv_ll_container);
		        mCvLlContainer2 = ma_i_view2;
		        mCvRbClick2 = (CheckBox) ma_i_view2.findViewById(R.id.cv_rb_click);
		        mCvTvTxt2 = (TextView) ma_i_view2.findViewById(R.id.cv_tv_txt);
		        mCvTvTxt2.setText("满100减20优惠券");
		
		        //第三个
		        // mCvLlContainer3 = (LinearLayout) ma_i_view3.findViewById(R.id.cv_ll_container);
		        mCvLlContainer3 = ma_i_view3;
		        mCvRbClick3 = (CheckBox) ma_i_view3.findViewById(R.id.cv_rb_click);
		        mCvTvTxt3 = (TextView) ma_i_view3.findViewById(R.id.cv_tv_txt);
		        mCvTvTxt3.setText("20优惠券");
		
		    }
		}





## 现场一面安卓、算法
* 自我介绍
* 我立刻提及之前电话面试没有答出来的retrofit的优势的问题，回答了这个问题。
* 介绍项目
* 详细介绍了我的爬虫项目。
* 问了昨晚做的那个自定义控件，如果想要在容器上监听响应事件怎么做？如何进行事件拦截？整个页面的事件流是怎么走的？
* 接着上面的问题，以上述控件为例讲一下事件分发。
* 适配怎么做的？
* 开发中优化怎么做的？
* 聊到了集合list的修改失败、内存泄露问题。
* 算法题:1，1，2，3，5，8，13……。求第n个数。（我采用了递归，后要求提高效率，想其他办法。在讨论提醒下写了出来）
* 算法题:在矩阵中，点上存在0或者1，若是0表示不通过，是1表示通过。AB两点为任意位置。当连接两个点上路径全部为1且只拐角一次则表示通路（连接两点有两条路可走，只拐一次），否则表示不通路。求AB两点是否通路。（写出来了，但是写的太复杂，技术说还行，算及格-_-||）

## 二面计算机基础、算法、产品、后端、数据库
* 自我介绍
* 产品介绍
* 又一遍详细介绍我的csdn+。
* 后端用到的什么
* 了解事务吗？事务的四大特点是什么？了解数据库引擎吗，都有哪几种，有什么特点？
* 你说你的csdn+中有用到数据库优化，怎么优化的？
* 你们后端的接口怎么处理的？
* 接口请求有哪几种？get和post有什么区别？
* 知道二分法吗？介绍一下。二分法的时间复杂度（答错了，挂了）。
* 我主动提出我们公司后端文件系统的处理，以及问题，以及解决方案。
有做过压力测试吗（这特么我在公司一个人做的。刚刚好！）？提到了数据库默认连接数的问题。
* 反问:咱们公司现在做哪方面的产品，具体服务对象是哪些？（深入聊了一段时间）
* 反问:技术部门的构成想要了解一下。
* 你计算机基础怎么样？数据库，计算机网络网络，操作系统，算法？（要求聊算法）
* 算法:自定义一个栈，完成入栈，出栈，找出最小数。要求时间复杂度为O（1），（本题挂了）
* 有什么想问的吗？（没有，刚问了）



## 三面人事面
* 之前在苏州工作，你说喜欢上海这边的技术氛围。还有其他的吗？这边有朋友吗？
* 现在住在哪，住的地安排好了吗？
* 聊了公司的投资方，介绍了国内几大风投，以及公司拿到的风投。老板的背景。目前公司的情况。
* 问我还有什么要了解的。又问了产品，公司规划，技术部门等。
* 你选择工作比较看中哪几点？（公司前景，技术氛围，薪资）。最看中哪一点，大概占？%。
* 你的期望薪资是多少？大概什么时候能上班？
* 结束。

## 注
> 最终没被录用，很不甘心。特别喜欢这个公司以及其团队。


#上海晨之科
## 现场面试
#### 问
安卓技术：
* 自我介绍
* 介绍csdn+（自己写的一个项目）
* 介绍材知府（公司项目）
* 项目中遇到什么问题，怎么解决？
* 说一下用到的第三方框架，库。了解其中的原理吗？（说了eventbus源码）
* 知道设计模式吗？（说了项目中用到的模板模式，单例模式，提到了双重锁的安全性问题和解决方案* * 以及eventbus用到了双重锁。）
* 介绍一下handler？
* 介绍一下安卓四种启动模式？
* 我问了公司技术部分的组织架构
* 我问了他们产品
* 我问了是否使用主流的rx，mvp，dragger2。（对方提到了用[Android-CleanArchitecture](https://github.com/android10/Android-CleanArchitecture "https://github.com/android10/Android-CleanArchitecture")架构，刚好我听过，聊了一下；说了为什么不用rx。）
* 对方主动介绍了现在他负责的产品以及如果我过去要做的哪一块。
* 对方主动介绍了sdk开发的注意事项以及框架的引入问题。（比如对于sdk开发特别在意版本的控制、包的大小、文档更新）
* 对方提到了glide根据context的生命周期去实现图片加载功能，并从glide中抽出来用到了自己的开发中。（比较好的是我看到了这一块，也提了一下。比较好）

人事:
> （我巴拉巴拉说了很多，他来一句我是人事，不太懂你们技术。男的，我以为是技术总监。。。）

* 现在住在哪
* 看你前两家工作时间都不是太长。（自言自语，直接过了）
* 我说了自己喜欢搞技术，兴趣比较广泛什么的。。。
* 说技术正在和老板交谈，稍后给你回复


#美团
## 问题
#### 2017.4.6号一面电话面试
* 自我介绍
* 泛型
	* 简单讲一下泛型
	* 泛型有什么作用
	* 运行期可以确定类型吗？

* 虚拟机
	* gc怎么判断对象已死亡
	* 可达性分析法怎么哪些对象可以作为根节点
	* 可达性分析法是树的形式不是图的形式

* Java可以多继承吗
* java接口和抽象类的区别
* hashmap底层数据结构、如何处理冲突
* hashset和hashmap的区别
* http和https的区别，算法实现？没答出来
* handler机制
* activityA跳转到B,生命周期；知道binder吗？应该是让讲原理的，没太明白。
* ActivityThread和ActivityMangerSystem机制（自己带入坑的，没答好）。
* 主动提出介绍一下项目架构
* handler的内存泄露问题
* 软引用和弱引用的区别
* Service的是在主线程运行还是在子线程运行?IntentService和Service的区别（自己主动引过来的）？
* 看过EventBus的源码吗？它是否存在内存泄露问题？有怎么样的处理机制？
* 混淆的问题？比如EventBus加入之后，如何做混淆。
* 在线写算法：对版本号排序，比如：1.2.0,2.3,3.22.1,1.0.3。（没写出来）
* 我又问题他三个问题，结束。

#### 2017.4.7号二面电话面试
* 不知道怎么聊起我的爬虫处理，提到怎么进行查询优化的
* 中间一直电话断断续续，好长时间，面试官换了几个地方==
* Set和list的区别，Set怎么处理重复数据的？
* hashmap底层实现，怎么解决冲突的？你会怎么解决hash冲突？
* 权限修饰符protected和默认的区别？proctected 修饰的方法，假如子类和父类不在一个包下，子类可以访问父类中这个方法吗？（没答出来！！！没想到会问这问题，就没复习）
* 静态代码块和构造方法哪个先执行？子类中的静态代码块和父类的静态代码块哪个先执行？
* String a="abc";String b="abc",创建了几个对象？
* tcp/ip三次握手，为什么必须三次，两次不行吗(原因没答好，特别不满意==)？
* http和https的区别和联系（直接不会。。。）。
* 了解图的遍历吗？（不了解。。。说了解树的遍历，不满意，问到了爬虫怎么处理的，不是类似图吗？）
* 手写算法：手写双重锁单例模式（syn...关键字没写出来，太长了，应该影响不大）
* 手写代码：一组double数据，取出最大和最小的数？（我提出用归并排，然后取。最后发现真傻逼，这样太麻烦了。写出来归并后，提出定义一个临时最小值，一直对比，对比结束既可以取出最小值。不知道面试官是不是要这个方法？应该不会这么简单）
* handler机制（特别简单那种）
* 进程优先级（前台进程、后台进程、服务进程等）
* IntentService和Service的区别？哪个优先级高？
* 问了面试官两个问题结束。

## 参考答案（个人见解）

#### 2017.4.6号一面电话面试
###### 自我介绍
###### 泛型
* 简单讲一下泛型
* 泛型有什么作用
* 运行期可以确定类型吗？

* 答：泛型是1.5新出的特性。主要可以在编译期校验参数类型，编译器可以更有效地提高Java程序的类型安全。但是，仅仅是在编译期进行类型验证，在运行期会进行“类型擦除”，所以在运行期这些泛型参数会全部被擦除掉。**运行期可以确定类型吗？**：比如`List<String>`,这个String在运行期可以确认吗？这个问题真的不知道，到现在不清楚。求解答。（根据面试官的反应应该是可以确认的）
* 另外，运用泛型除了解决校验参数外，可以设计一些比较优雅的代码，比如设计一个如下的类，用于服务端到客户端请求bean的封装：
	
		class bady<T>{
			public int code;//状态码
			public String msg;//附加信息
			public T data;//数据
		}

###### 虚拟机
* gc怎么判断对象已死亡
* 可达性分析法怎么哪些对象可以作为根节点
* 可达性分析法是树的形式不是图的形式
* 答：1、引用计数法；2、可达性分析法。在主流虚拟机中都不会采用第一种方法，因为它难以解决对象间循环引用的问题。可以作为根节点的对象：1、虚拟机栈中引用的对象；2、本地方法栈中引用的对象3、方法区中类静态属性引用的对象；4、方法区中常量引用的对象；

###### Java可以多继承吗
* 不可以。

###### java接口和抽象类的区别
* 接口中只可以有抽象方法，可以被多实现。成员变量只可以是静态且final的，不管加不加static和final；抽象类是接口的一种折中处理，具有和接口的特性又具有普通类的特性，可以存在抽象方法和普通方法。成员变量只可以是静态的，不管加不加static。是否是final开发者可以自己定义。
* 为什么接口和抽象类成员变量必须是静态的？因为接口和抽象类不能实例化，所以成员变量是属于类的，所以应该是静态的。【个人见解】
* 为什么接口的成员变量是必须是final的，考虑到接口的特殊性，是定义一种行为的，如果任由实现类修改成员变量，会失去“通用性”。【个人见解】
* jdk1.8中对这一块有改变，接口也可以有静态方法以及默认方法。

###### hashmap底层数据结构、如何处理冲突
* 答：底层实现是数组加链表实现。解决冲突：拉链法，当命中到同一位置之后，会以链表的形式向后面链接。另外：默认的负载因子为0.75而不是1也是为了解决冲突的，增加命中的效率。除了拉链法解决冲突，还有线性探测法、伪随机数法等。特别的，1.8中有对这一块有扩展，当链表过长是，会转化为红黑树，增加查询的效率。

###### hashset和hashmap的区别
* 一个是set集合，一个是K、V对。（这特么问的。。。）

###### http和https的区别，算法实现？没答出来
* 客户端在使用HTTPS方式与Web服务器通信时有以下几个步骤，如图所示。
1. 客户使用https的URL访问Web服务器，要求与Web服务器建立SSL连接。
2. Web服务器收到客户端请求后，会将网站的证书信息（证书中包含公钥）传送一份给客户端。
3. 客户端的浏览器与Web服务器开始协商SSL连接的安全等级，也就是信息加密的等级。
4. 客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。
5. Web服务器利用自己的私钥解密出会话密钥。
6. Web服务器利用会话密钥加密与客户端之间的通信。
![](http://pic002.cnblogs.com/images/2012/339704/2012071410212142.gif)

* https缺点：
1. HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；
2. HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；
3. SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。
4. SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。
5. HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

* 大致算法实现：**浏览器采用对称加密，服务端采用非对称加密**，服务端发送公钥给浏览器--->浏览器运用公钥将私钥进行加密传输给服务端--->服务端运用私钥将公钥解密--->这个时候浏览器和服务器同时拥有浏览器生成的对称加密秘钥，进行通信。
* @see理解SSL(https)中的对称加密与非对称加密：[http://blog.csdn.net/mooncom/article/details/60140372](http://blog.csdn.net/mooncom/article/details/60140372)
* @see对称加密和非对称加密的解释：[http://www.cnblogs.com/jfzhu/p/4020928.html](http://www.cnblogs.com/jfzhu/p/4020928.html)

###### handler机制
* 由四大块组成：handler、mssage、massageQueue、Looper。
* massage是消息的载体，在子线程中通过handler进行消息的发送，首先会将消息发送到主线程MQ中进行排队，在主线程中Looper会一直进行循环检查MQ中是否有消息，有的话取出，然后通过handler在主线程中进行相应的处理。

###### activityA跳转到B,生命周期；知道binder吗？应该是让讲原理的，没太明白。
* AonPasue()--->BonCreate()--->BonStart()--->BonResume()--->AonStop()
* 这里引出一个问题，不要在onPasue()中做耗时操作，不然会影响B的启动速度。那么：如何加快应用的冷启动呢？
* binder原理：。。。

###### ActivityThread和ActivityMangerSystem机制（自己带入坑的，没答好）。
* 求解答

###### 主动提出介绍一下项目架构
* 客户端采用核心业务module+与业务无关的lib+各种第三方库的形式进行组织。
* 我们对业务无关的自定义view、utils、适配器的封装、基类的封装（activity、fragment）全部放lib中。依赖方式为单向依赖，核心业务只单向依赖lib，保证了模块之间的低耦合。
* 对于重复性的动作，比如EventBus的注册和反注册、ButterKnife的绑定和反绑定、tag的获取、Activity的入栈等重复动作全部放到基类中进行实现，避免了重复性动作。当然，一些业务必须需要延迟到子类中进行实现，比如是否需要注册EventBus,这里采用**模板模式**将是否注册延迟到子类进行实现。
* 特别的，对于封装的基类，如果我们想要添加和业务相关的东西，可能会破坏模块之间的单向依赖关系。所以在使用的使用，又在业务module中实现了base类。
* 核心业务分为以下几大块：base模块（对lib中基类进行了继承和重写）、api模块（负责网络请求）、event模块（EventBus通知对象类）、bean模块、utils模块、ui（模块）；ui下又包括activity、fragment、adapter模块；每个子模块下又包括不同的业务模块。

###### handler的内存泄露问题
* 静态内部类和非静态内部类的区别：非静态内部类会持有外部类的引用，所以当内部类的生命周期大于外部类时，会导致外部类无法被gc回收。handler是一个不同类，满足上述叙述。
* 解决上述问题：将内部类生命为静态类型。由于Handler不再持有外部类对象的引用，导致程序不允许你在Handler中操作Activity中的对象了。所以你需要在Handler中增加一个对Activity的弱引用（WeakReference）：

		static class MyHandler extends Handler {
		    WeakReference<Activity > mActivityReference;
		
		    MyHandler(Activity activity) {
		        mActivityReference= new WeakReference<Activity>(activity);
		    }
		
		    @Override
		    public void handleMessage(Message msg) {
		        final Activity activity = mActivityReference.get();
		        if (activity != null) {
		            mImageView.setImageBitmap(mBitmap);
		        }
		    }
		}

* 当Activity finish后 handler对象还是在Message中排队。还是会处理消息，这些处理有必要？正常Activitiy finish后，已经没有必要对消息处理，那需要怎么做呢？解决方案也很简单，在Activity onStop或者onDestroy的时候，取消掉该Handler对象的Message和Runnable。通过查看Handler的API，它有几个方法：removeCallbacks(Runnable r)和removeMessages(int what)等。

###### 软引用和弱引用的区别
* 简单来说，软引用的回收时机是在内存不够用时，gc才会回收被软引用修饰的的对象；弱引用回收的时机是在下一次触发gc时，而不管内存是否够用。

###### Service的是在主线程运行还是在子线程运行?IntentService和Service的区别（自己主动引过来的）？
* Service是运行在主线程中的，因而也会出现所谓ARN，不过时长较长，大概为20S。
* IntentService是继承于Service并处理异步请求的一个类，在IntentService内有一个工作线程来处理耗时操作，启动IntentService的方式和启动传统Service一样，同时，当任务执行完后，IntentService会自动停止，而不需要我们去手动控制。另外，可以启动IntentService多次，而每一个耗时操作会以工作队列的方式在IntentService的onHandleIntent回调方法中执行，并且，每次只会执行一个工作线程，执行完第一个再执行第二个，以此类推。内部通过**HanlderThread**处理线程之间的交互工作。
* HanderThread类继承了Thread类，它封装了 Looper 对象，使我们不用关心 Looper 的开启和释放的细节问题(因为 Looper 的构造函数是私有的，对于其实例的获取比较麻烦，而 HandlerThread 帮我搞定了这些繁琐)。HandlerThread 对象中可以通过 getLooper 方法获取一个 Looper 对象引用。

###### 看过EventBus的源码吗？它是否存在内存泄露问题？有怎么样的处理机制？
* 注册过程，EventBus执行EventBus.getDefault().register(Object obj)，首先getDefault()方法是一个以双重锁的方式获取EventBus单例，保证在安全的前提下获取EventBus实例。Register(Object obj)执行一个重载方法，最终传递四个参数：注册对象、@Subscribe方法名、是否黏贴、线程优先级。然后通过一个方法findSubscriberMethods()传递两个参数当前对象、@Subscribe方法名去遍历当前类，将所有@Subscribe方法加入到一个List<SubscriberMethod>中，在SubscriberMethod方法中包括三个属性：method(方法名), threadMode(模式类型), eventType(参数类型，也就是要传递对象)。拿到集合之后然后遍历集合，根据subscriberMethod.eventType，去subscriptionsByEventType去查找一个CopyOnWriteArrayList<Subscription> ，如果没有则创建。顺便把我们的传入的参数封装成了一个：Subscription（subscriber, subscriberMethod, priority）；
这里的subscriptionsByEventType是个Map，key：eventType ； value：CopyOnWriteArrayList<Subscription> 。简而言之：就是一个Map<K,V>,K指的是onEvent(Object obj)中的obj这个对象，V指的是Subscription，包含：subscriber(订阅者), subscriberMethod(@Subscribe方法), priority(优先级)。那么，走到这里，可以猜测post(Object obj)发送消息的时候，会去以obj为Key进行遍历Map然后取得List,然后遍历List，去一个一个通知所对应的方法。
* Post过程，post首先会执行一个List.add()方法，将当前请求加入到一个队列中。然后遍历List取得该对象，执行一个postSingleEvent(Object event, PostingThreadState postingState)的方法，根据event（也就是Map中的key）获取一个List集合，然后依次去调用@Subscribe方法去实现消息的发送。Post过程比较简单，核心逻辑在register。
* 使用的时候必须在onDestory()方法中进行反注册，不然会导致内存泄露。查看源码，会根据当前对象去subscriptionsByEventType找到 List<Subscription> subscriptions，将所有和注册对象相关的Subscription全部移除掉。

###### 混淆的问题？比如EventBus加入之后，如何做混淆。
* 混淆处理的好处
	* 压缩，会移除未被使用的类和成员
	* 优化，在字节码级别的执行优化
	* 混淆，增大反编译的难度；并且，一般混淆后各种字段都会变短，也起到了压缩的作用。

* 注意，有些类和方法不能被混淆
	* 原生API不要混淆，也没必要混淆
	* native不能混淆
	* 反射用到的类最好不要混淆
	* Parcelable子类不能混淆

* 第三方库的混淆规则参考对应的官方文档即可。

###### 在线写算法（记事本形式）：对版本号排序，比如：1.2.0,2.3,3.22.1,1.0.3。（没写出来）
* 一个思路，以数组的形式进行自定义排序。从左向右比较，实现很巧妙。我是想不到。。。**求解答**（其它思路）

		public class VersionSort {
			public static void main(String[] args) {
		
				Scanner scanner = new Scanner(System.in);
				while (scanner.hasNextInt()) {
					int n = scanner.nextInt();
					String[] nums = new String[n];
					for (int i = 0; i < n; i++) {
						nums[i] = scanner.next();
					}
					Arrays.sort(nums, new Comparator<String>() {
						@Override
						public int compare(String o1, String o2) {
							String num1[] = o1.split("\\.");
							String num2[] = o2.split("\\.");
							int i = 0;
							for (i = 0; i < num1.length && i < num2.length; i++) {
								int ll = Integer.parseInt(num1[i]);
								int rr = Integer.parseInt(num2[i]);
								if (ll == rr)
									continue;
								if (ll < rr)
									return -1;
								else
									return 1;
							}
							if (i < num1.length)
								return 1;
							if (i < num2.length)
								return -1;
							return 0;
						}
					});
					for (String s : nums)
						System.out.println(s);
				}
		
			}
		
		}



###### 我又问题他三个问题，结束。

#### 2017.4.7号二面电话面试
###### 不知道怎么聊起我的爬虫处理，提到怎么进行查询优化的
* 索引，对于经常查询的字段加上必要的索引。比如进行csdn所有用户爬取过程中，加入数据库之前需要查询该用户id是否在数据库，这里加不加索引影响十分明显。
* 拆分表，单独建立一张爬取所有用户的表（spider_user），只有id、userid、createTime三个字段，降表的逻辑，增加爬取效率。对于其它信息，会在建立任务，去根据spider_user再去爬取详细信息。
* 减少数据库IO,将数据量较多的字段，比如全文，甚至文章的描述信息（大概100字左右）以文件的形式存储，而不是以数据库字段的形式进行存储。十分明显的，如果将文件的描述信息存储到数据库，在数据量20W左右的时候，数据库预览基本就打不开；然而去除掉该字段预览数据库秒开。
* 在内存级别优化，自建Set集合，将爬取过的页面存放进去。在下次爬取的过程中，对比，存在直接跳过。注意，尽可能的扩大初始容量。

###### 中间一直电话断断续续，好长时间，面试官换了几个地方==
###### Set和list的区别，Set怎么处理重复数据的？
* 区别：set只能存放不相同的处理；list可以存放相同的对象。
* 通过equal()方法和hashCode()方法确认是否是同一个对象。

###### hashmap底层实现，怎么解决冲突的？你会怎么解决hash冲突？
* 数组+链表
* 通过对象的hash值对比，如果不存在，将数据放到该处；如果已经存在该值，会在该对象后面以链表的形式进行链接。1.8之后对于链表过长（18左右）会转化为红黑树，原因是因为链表的查询效率过低。
* 拉链法、线性探测法、随机数法、二次探测法（若当前key与原来key产生相同的哈希地址，则当前key存在该地址后偏移量为（1,2,3...）的二次方地址处）

###### 权限修饰符protected和默认的区别？proctected 修饰的方法，假如子类和父类不在一个包下，子类可以访问父类中这个方法吗？（没答出来！！！没想到会问这问题，就没复习）
> 在不同包下面能够访问的权限修饰符只有： pulbic 与protected，但是 protected 必须要有继承的关系才能够访问。

特别的：
* **默认权限**的只要不在同一包下均不能访问，不管是否有继承关系
* protected 如果有继承关系，在不同包下也可以访问；没有继承关系不能访问。

![](http://img.blog.csdn.net/20160624180338588)

###### 静态代码块和构造方法哪个先执行？子类中的静态代码块和父类的静态代码块哪个先执行？
* 静态代码块属于类，构造方法属于实例，所以静态代码块先执行。
* 父类的构造方法早于子类的构造方法执行。
* 类比，父类的静态代码块也早于子类先执行。
* Son son=new Son(),测试代码如下

		Father的静态代码块
		Son的静态代码块
		Father的构造方法
		Son的构造方法

###### String a="abc";String b="abc",创建了几个对象？

* 首先明确一点，这一块很复杂，没有找到特别权威的答案，只提供一个大致思路；并且这种面试题一直**被吐槽**，没有任何意义。如果能把下面的问题说清楚即可，答案有依据即可。
> 原理1：当使用任何方式来创建一个字符串对象s时，Java运行时（运行中JVM）会拿着这个X在String池中找是否存在内容相同的字符串对象，如果不存在，则在池中创建一个字符串s，否则，不在池中添加。
原理2：Java中，只要使用new关键字来创建对象，则一定会（在堆区或栈区）创建一个新的对象。
原理3：使用直接指定或者使用纯字符串串联来创建String对象，则仅仅会检查维护String池中的字符串，池中没有就在池中创建一个，有则罢了！但绝不会在堆栈区再去创建该String对象。
原理4：使用包含变量的表达式来创建String对象，则不仅会检查维护String池，而且会在堆栈区创建一个String对象。**返回的是堆上的对象**（原文没这一句话，猜测的，并且测试确实是这样）。

* 针对上述四大原理进行测试，全部通过。是否可信请斟酌。

![这里写图片描述](http://img.blog.csdn.net/20170419094329828?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2d5c2NzZg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* `String a="abc";String b="abc",创建了几个对象？`，，一次，只在String池中创建一次。
* `String a=new String ("abc");String b=new String("abc");`,三次，String池中一次，堆中两次。
* `String a="abc";String b="a"+"bc";`一次，只在String池中创建一次。
* `String a="a";String b=a+"bc";`三次，String a="a"在String池中创建一次，String b=a+"bc"在常量池中一次，在堆中一次。

###### tcp/ip三次握手，为什么必须三次，两次不行吗(原因没答好，特别不满意==)？
* 三次握手：客户端发送SYN连接请求--->服务端发送ACK确认请求+SYN连接请求---->客户端发送ACK确认连接。
* 为什么必须三次，两次不行吗？采用三次握手是为了防止失效的连接请求报文段突然又传送到主机B，因而产生错误。失效的连接请求报文段是指：主机A发出的连接请求没有收到主机B的确认，于是经过一段时间后，主机A又重新向主机B发送连接请求，且建立成功，顺序完成数据传输。考虑这样一种特殊情况，主机A第一次发送的连接请求并没有丢失，而是因为网络节点导致延迟达到主机B，主机B以为是主机A又发起的新连接，于是主机B同意连接，并向主机A发回确认，但是此时主机A根本不会理会，主机B就一直在等待主机A发送数据，导致主机B的资源浪费。
* 四次挥手：客户端发送FIN关闭连接请求--->服务端发送ACK确认请求--->服务端发送FIN请求--->客户端发送ACK确认请求。

###### http和https的区别和联系（直接不会。。。）。
* 不是简单的一个安全一个不安全哦~，一面已经对这个问题进行了解释。

###### 了解图的遍历吗？（不了解。。。说了解树的遍历，不满意，问到了爬虫怎么处理的，不是类似图吗？）
* 不知道如何阐述

###### 手写算法：手写双重锁单例模式（syn...关键字没写出来，太长了，应该影响不大）
* 很简单
* 请注意双重锁的安全（反射，不是指的并发）问题，可以采用枚举解决安全问题。

###### 手写代码：一组double数据，取出最大和最小的数？（我提出用归并排，然后取。最后发现真傻逼，这样太麻烦了。写出来归并后，提出定义一个临时最小值，一直对比，对比结束既可以取出最小值。不知道面试官是不是要这个方法？应该不会这么简单）
* 有坑吗？

		double min=-1000;
		for(int i=0;i<arr.length;i++)
			if(arr[i]<min)
				min=arr[i];
		
		reslut:min;

###### handler机制（特别简单那种）
* 一面已回答


###### 进程优先级（前台进程、后台进程、服务进程等）

按优先级从高到底的顺序：

1. Foreground processes 前台进程
	* 进程中包含处于前台的正与用户交互的activity;
	* 进程中包含与前台activity绑定的service;
	* 进程中包含调用了startForeground()方法的service;
	* 进程中包含正在执行onCreate(), onStart(), 或onDestroy()方法的service;
	* 进程中包含正在执行onReceive()方法的BroadcastReceiver.

2. Visiable processes 可视进程
	* 进程中包含未处于前台但仍然可见的activity(调用了activity的onPause()方法, 但没有调用onStop()方法)。 典型的情况是：运行activity时弹出对话框（类似对话框，将activity遮挡）, 此时的activity虽然不是前台activity, 但其仍然可见。
	* 进程中包含与可见activity绑定的service.可视进程不会被系统杀死, 除非为了保证前台进程的运行而不得已为之.
3. Service processes 服务进程
	* 正在运行的Service（不在create(),start(),destory()状态中）
4. background processes 后台进程
	* 如：不可见状态的activity
5. Empty processes 空进程
	* 不包含任何处于活动状态的进程是一个空进程. 系统经常杀死空进程, 这不会造成任何影响. 空进程存在的唯一理由是为了缓存一些启动数据, 以便下次可以更快的启动.

###### IntentService和Service的区别？哪个优先级高？
* 一个在子线程运行一个在主线程运行。IntentService可以做耗时操作。
* 哪个优先级高：不懂


###### 问了面试官两个问题结束。

