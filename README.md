# designPattern 设计模式总纲
design pattern code 
本文作者左潇龙  http://www.cnblogs.com/zuoxiaolong/p/pattern1.html

最近一直在学习设计模式相关的知识，还是老规矩，和各位一起学习，一起探讨，本系列所发表所有内容仅代表个人观点。

 

《简介》
    说到设计模式，当初第一次听到时，第一反应就是很深奥，完全理解不了这个概念到底是什么意思，下面我先从网上摘录一份定义。

    设计模式（Designpattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。

    上面是百度当中的解释，来解释一下这句简单的话的含义，几个关键词。

    反复使用：这个不用过多解释，设计模式被使用太多了，上个系列spring源码当中就出现了很多模式，记忆中比较深刻的有模板模式，代理模式，单例模式，工厂模式等等。

    多数人知晓：这个就不需要过多解释了。

    分类编目：就是说可以找到一些特征去划分这些设计模式，从而进行分类。

    代码设计经验：这句很重要，设计经验的总结，也就是说设计模式，是为了指导设计而从经验中总结出来的套路。

    还有一种说法是说，设计模式是可以解决特定场景的问题的一系列方法，其实我觉得这个解释更贴切一点。

       

《为何学习设计模式》
 

       上面简单的介绍，是让各位首先搞清楚设计模式是什么，下面我们来说说为什么要学习设计模式，学习总要有个驱动力。

       有过工作经验的人都知道，特别是那些在维护一个项目的人更是体会的贴切，像我就是其中一个，有的时候，一个很简单的需求，或者说，本来应该是很快就可以实现的需求，但是由于系统当初设计的时候没有考虑这些需求的变化，或者随着需求的累加，系统越来越臃肿，导致随便修改一处都可能造成不可预料的后果，或者是我本来可以修改下配置文件或者改一处代码就可以解决的事情，结果需要修改N处代码才可以达到我的目的。

       以上都是非常可怕的后果，这些我已经深深体会过了。

 

《设计模式的好处及注意点》
 

       设计模式可以帮助我们改善系统的设计，增强系统的健壮性、可扩展性，为以后铺平道路。

       但是，这些是我当初第一次接触设计模式时的感受，现在我并不这么认为，设计模式可以改善系统的设计是没错，但是过多的模式也会系统变的复杂。所以当我们第一次设计一个系统时，请将你确定的变化点处理掉，不确定的变化点千万不要假设它存在，如果你曾经这么做过，那么请改变你的思维，让这些虚无的变化点在你脑子中彻底消失。

       因为我们完全可以使用另外一种手法来容纳我们的变化点，那就是重构，不过这是我们在讨论过设计模式之后的事情，现在我们就是要把这些设计模式全部理解，来锻炼我们的设计思维，而不是只做一个真正的码农。

 

《指导原则：六大规则》
 

       在学习设计模式之前，为了不让设计模式显得很模式，我们还必须了解一个东西，那就是程序设计六大原则。

       这些原则是指导模式的规则，我会给一些原则附上一个例子，来说明这个原则所要表达的意思，注意，原则是死的，人是活的，所以并不是要你完完全全遵守这些规则，否则为何数据库会有逆范式，只是在可能的情况下，请尽量遵守。

 

       单一职责原则（六大规则中的小萝莉，人见人爱）：描述的意思是每个类都只负责单一的功能，切不可太多，并且一个类应当尽量的把一个功能做到极致。

       否则当你去维护这个系统的时候，你会后悔你当初的决定，下面是我自己思索的例子，给各位参考一下，给出代码。

复制代码
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;


public class Calculator {

    public int add() throws NumberFormatException, IOException{
        File file = new File("E:/data.txt");
        BufferedReader br = new BufferedReader(new FileReader(file));
        int a = Integer.valueOf(br.readLine());
        int b = Integer.valueOf(br.readLine());
        return a+b;
    }
    
    public static void main(String[] args) throws NumberFormatException, IOException {
        Calculator calculator = new Calculator();
        System.out.println("result:" + calculator.add());
    }
}
复制代码
          来看上面这个例子，这个方法的作用是从一个文件中读出两个数，并返回它们的和，我相信各位也能看出当中有明显的多职责问题。如果没看出来的话，我想问各位一句，如果我想算这个文件中两个数字的差该如何做？

          相信答案应该是我COPY出来一个div方法，把最后的加号改成减号。好吧，那我要除法呢？乘法呢？取模呢？COPY四次吗。这就造成了很多很多的代码重复，这不符合系统设计的规则。下面我把上述程序改善一下。

          我们分离出来一个类用来读取数据，来看Reader。

复制代码
package com.test;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;


public class Reader {
    
    int a,b;
    
    public Reader(String path) throws NumberFormatException, IOException{
        BufferedReader br = new BufferedReader(new FileReader(new File(path)));
        a = Integer.valueOf(br.readLine());
        b = Integer.valueOf(br.readLine());
    }
    
    public int getA(){
        return a;
    }
    
    public int getB(){
        return b;
    }
}
复制代码
          下面是我们单独的计算器类。

复制代码
package com.test;
import java.io.IOException;


public class Calculator {

    public int add(int a,int b){
        return a + b;
    }
    
    public static void main(String[] args) throws NumberFormatException, IOException {
        Reader reader = new Reader("E:/data.txt");
        Calculator calculator = new Calculator();
        System.out.println("result:" + calculator.add(reader.getA(),reader.getB()));
    }
    
}
复制代码
        我们将一个类拆成了两个类，这样以后我们如果有减法，乘法等等，就不用出现那么多重复代码了。

        以上是我临时杜撰的例子，虽然很简单，并且没有什么现实意义，但是我觉得足够表达单一职责的意思，并且也足够说明它的重要性。单一职责原则是我觉得六大原则当中最应该遵守的原则，因为我在实践过程中发现，当你在项目的开发过程中遵循它，几乎完全不会给你的系统造成任何多余的复杂性，反而会令你的程序看起来井然有序。

 

        里氏替换原则（六大原则中最文静的姑娘，但却不太招人喜欢）：这个原则表达的意思是一个子类应该可以替换掉父类并且可以正常工作。

        那么翻译成比较容易理解的话，就是说，子类一般不该重写父类的方法，因为父类的方法一般都是对外公布的接口，是具有不可变性的，你不该将一些不该变化的东西给修改掉。

        上述只是通常意义上的说法，很多情况下，我们不必太理解里氏替换这个文静的姑娘，比如模板方法模式，缺省适配器，装饰器模式等一些设计模式，就完全不搭理这个文静的姑娘。

        不过就算如此，如果你真的遇见了不得不重写父类方法的场景，那么请你考虑，你是否真的要把这个类作为子类出现在这里，或者说这样做所换来的是否能弥补你失去的东西，比如子类无法代替父类工作，那么就意味着如果你的父类可以在某一个场景里工作的很正常，那么你的子类当然也应该可以，否则就会出现下述场景。

        比如我们有某一个类，其中有一个方法，调用了某一个父类的方法。

复制代码
//某一个类
public class SomeoneClass {
    //有某一个方法，使用了一个父类类型
    public void someoneMethod(Parent parent){
        parent.method();
    }
}
复制代码
           父类代码如下。

public class Parent {

    public void method(){
        System.out.println("parent method");
    }
}
           结果我有一个子类把父类的方法给覆盖了，并且抛出了一个异常。

复制代码
public class SubClass extends Parent{

    //结果某一个子类重写了父类的方法，说不支持该操作了
    public void method() {
        throw new UnsupportedOperationException();
    }
    
}
复制代码
          这个异常是运行时才会产生的，也就是说，我的SomeoneClass并不知道会出现这种情况，结果就是我调用下面这段代码的时候，本来我们的思维是Parent都可以传给someoneMethod完成我的功能，我的SubClass继承了Parent，当然也可以了，但是最终这个调用会抛出异常。

复制代码
public class Client {

    public static void main(String[] args) {
        SomeoneClass someoneClass = new SomeoneClass();
        someoneClass.someoneMethod(new Parent());
        someoneClass.someoneMethod(new SubClass());
    }
}
复制代码
           这就相当于埋下了一个个陷阱，因为本来我们的原则是，父类可以完成的地方，我用子类替代是绝对没有问题的，但是这下反了，我每次使用一个子类替换一个父类的时候，我还要担心这个子类有没有给我埋下一个上面这种炸弹。

           所以里氏替换原则是一个需要我们深刻理解的原则，因为往往有时候违反它我们可以得到很多，失去一小部分，但是有时候却会相反，所以要想做到活学活用，就要深刻理解这个原则的意义所在。      

     
          接口隔离原则（六大原则当中最挑三拣四的挑剔女，胸部极小）：也称接口最小化原则，强调的是一个接口拥有的行为应该尽可能的小。

          如果你做不到这一点你经常会发现这样的状况，一个类实现了一个接口，里面很多方法都是空着的，只有个别几个方法实现了。

          这样做不仅会强制实现的人不得不实现本来不该实现的方法，最严重的是会给使用者造成假象，即这个实现类拥有接口中所有的行为，结果调用方法时却没收获到想要的结果。

          比如我们设计一个手机的接口时，就要手机哪些行为是必须的，要让这个接口尽量的小，或者通俗点讲，就是里面的行为应该都是这样一种行为，就是说只要是手机，你就必须可以做到的。

          上面就是接口隔离原则这个挑剔女所挑剔的地方，假设你没有满足她，你或许会写出下面这样的手机接口。

复制代码
public interface Mobile {

    public void call();//手机可以打电话
    
    public void sendMessage();//手机可以发短信
    
    public void playBird();//手机可以玩愤怒的小鸟？
    
}
复制代码
           上面第三个行为明显就不是一个手机应该有的，或者说不是一个手机必须有的，那么上面这个手机的接口就不是最小接口，假设我现在的非智能手机去实现这个接口，那么playBird方法就只能空着了，因为它不能玩。

           所以我们更好的做法是去掉这个方法，让Mobile接口最小化，然后再建立下面这个接口去扩展现有的Mobile接口。

public interface SmartPhone extends Mobile{

    public void playBird();//智能手机的接口就可以加入这个方法了
    
}
         这样两个接口就都是最小化的了，这样我们的非智能手机就去实现Mobile接口，实现打电话和发短信的功能，而智能手机就实现SmartPhone接口，实现打电话、发短信以及玩愤怒的小鸟的功能，两者都不会有多余的要实现的方法。

         最小接口原则一般我们是要尽量满足的，如果实在有多余的方法，我们也有补救的办法，而且有的时候也确实不可避免的有一些实现类无法全部实现接口中的方法，这时候就轮到缺省适配器上场了，这个在后面再介绍。

 

         依赖倒置原则（六大原则中最小鸟依人的姑娘，对抽象的东西非常依赖）：这个原则描述的是高层模块不该依赖于低层模块，二者都应该依赖于抽象，抽象不应该依赖于细节，细节应该依赖于抽象。

         上面黑色加粗这句话是这个原则的原版描述，我来解释下我自己的理解，这个原则描述的是一个现实当中的事实，即实现都是易变的，而只有抽象是稳定的，所以当依赖于抽象时，实现的变化并不会影响客户端的调用。

         比如上述的计算器例子，我们的计算器其实是依赖于数据读取类的，这样做并不是很好，因为如果我的数据不是文件里的了，而是在数据库里，这样的话，为了不影响你现有的代码，你就只能将你的Reader类整个改头换面。

         或者还有一种方式就是，你再添加一个DBReader类，然后把你所有使用Reader读取的地方，全部手动替换成DBReader，这样其实也还可以接受，那假设我有的从文件读取，有的从数据库读取，有的从XML文件读取，有的从网络中读取，有的从标准的键盘输入读取等等。

         你想怎么办呢？

         所以我们最好的做法就是抽象出一个抽象类或者是接口，来表述数据读取的行为，然后让上面所有的读取方式所实现的类都实现这个接口，而我们的客户端，只使用我们定义好的接口，当我们的实现变化时，我只需要设置不同的实际类型就可以了，这样对于系统的扩展性是一个大大的提升。

         针对上面简单的数据读取，我们可以定义如下接口去描述。

public interface Reader {

    public int getA();
    
    public int getB();
}
       让我们原来的Reader改名为FileReader去实现这个接口，这样计算器就依赖于抽象的接口，这个依赖是非常稳定的，因为不论你以后要从哪读取数据，你的两个获取数据的方法永远都不会变。

         这样，我们让DBReader，XMLReader，NETReader，StandardOutPutStreamReader等等，都可以实现Reader这个接口，而我们的客户端调用依赖于一个Reader，这样不管数据是从哪来的，我们都可以应对自如，因为我根本不关心你是什么Reader，我只知道你能让我获得A和B这两个值就行了。

         这便是我们依赖于抽象所得到的灵活性，这也是JAVA语言的动态特性给我们带来的便利，所以我们一定要好好珍惜这个依赖于抽象的姑娘。

 

         迪米特原则（六大原则中最害羞的姑娘，不太爱和陌生人说话）：也称最小知道原则，即一个类应该尽量不要知道其他类太多的东西，不要和陌生的类有太多接触。

         这个原则的制定，是因为如果一个类知道或者说是依赖于另外一个类太多细节，这样会导致耦合度过高，应该将细节全部高内聚于类的内部，其他的类只需要知道这个类主要提供的功能即可。

         所谓高内聚就是尽可能将一个类的细节全部写在这个类的内部，不要漏出来给其他类知道，否则其他类就很容易会依赖于这些细节，这样类之间的耦合度就会急速上升，这样做的后果往往是一个类随便改点东西，依赖于它的类全部都要改。

         比如我把上述的例子改变一下。

复制代码
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;


public class Reader {
    
    int a,b;
    
    private String path;
    
    private BufferedReader br;
    
    public Reader(String path){
        this.path = path;
    }
    
    public void setBufferedReader() throws FileNotFoundException{
        br = new BufferedReader(new FileReader(new File(path)));
    }
    
    public void readLine() throws NumberFormatException, IOException{
        a = Integer.valueOf(br.readLine());
        b = Integer.valueOf(br.readLine());
    }
    
    public int getA(){
        return a;
    }
    
    public int getB(){
        return b;
    }
}
复制代码
         Reader类改成上述这个样子，显然它给其他的类透漏了太多细节，让别人知道了它的太多细节，这样我客户端调用的时候就很可能写成如下形式。

复制代码
public class Client {

    public static void main(String[] args) throws Exception {
        Reader reader = new Reader("E:/test.txt");
        reader.setBufferedReader();
        reader.readLine();
        int a = reader.getA();
        int b = reader.getB();
        //以下用于计算等等
    }
}
复制代码
           这样客户端就依赖于reader的多个行为才能最终获取到A和B两个数值，这时候两个类的耦合度就太高了，我们更好的做法使用访问权限限制将二者都给隐藏起来不让外部调用的类知道这些。就像下面这样。

复制代码
public class Reader {

    int a,b;
    private String path;
    private BufferedReader br;
    public Reader(String path) throws Exception{
        super();
        this.path = path;
        setBufferedReader();
        readLine();
    }
    //注意，我们变为私有的方法
    private void setBufferedReader() throws FileNotFoundException{
        br = new BufferedReader(new FileReader(path));
    }
    //注意，我们变为私有的方法
    private void readLine() throws NumberFormatException, IOException{
        a = Integer.valueOf(br.readLine());
        b = Integer.valueOf(br.readLine());
    }
    
    public int getA(){
        return a;
    }
    
    public int getB(){
        return b;
    }
}
复制代码
           我们最终将两个方法都变为私有封装在Reader类当中，这样外部的类就无法知道这两个方法了，所以迪米特原则虽说是指的一个类应当尽量不要知道其他类太多细节，但其实更重要的是一个类应当不要让外部的类知道自己太多。两者是相辅相成的，只要你将类的封装性做的很好，那么外部的类就无法依赖当中的细节。

 

           开-闭原则（六大原则中绝对的大姐大，另外五姐妹心甘情愿臣服）：最后一个原则，一句话，对修改关闭，对扩展开放。

           就是说我任何的改变都不需要修改原有的代码，而只需要加入一些新的实现，就可以达到我的目的，这是系统设计的理想境界，但是没有任何一个系统可以做到这一点，哪怕我一直最欣赏的spring框架也做不到，虽说它的扩展性已经强到变态。

           这个原则更像是前五个原则的总纲，前五个原则就是围着它转的，只要我们尽量的遵守前五个原则，那么设计出来的系统应该就比较符合开闭原则了，相反，如果你违背了太多，那么你的系统或许也不太遵循开闭原则。

 

           在《大话设计模式》一书中，提到一句话与各位共勉，我觉得很有说服力，即用抽象构建框架，用细节实现扩展。

           以上六个原则写出来是为了指导后面设计模式的描述，基本都是我自己体会出来的理解，或许当中有好有坏，有优有差，各位不需要太过在意形式的表述，完全可以有自己的理解。
