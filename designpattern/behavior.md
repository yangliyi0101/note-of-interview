## 策略模式
策略类接口 + 多个具体策略类
外加策略类Factory，把各个策略类放到map里，根据一个string来选择对应的策略类。

    @Component
    public class DatasetInfoStrategyFactory {

        @Autowired
        Map<String, DataSetInfoParseStrategy> dataSetInfoParseStrategyMap;

        public DataSetInfoParseStrategy getParser(String parser) {
            return dataSetInfoParseStrategyMap.get(parser);
        }
    }

## 模板模式
模板模式和策略模式很像。</br>
先从一个例子引入：泡茶与泡咖啡的例子。</br>
泡茶的流程是：烧水，拿茶壶，放入茶叶，冲泡；</br>
泡咖啡的流程：烧水，拿咖啡壶，放入可可，冲泡。</br>
二者的流程是不是非常接近？，难道我们要写两个类去区分他们？当然不是，模板模式就是为了解决这个问题而存在的。让我们抽象这个过程：
     
     
    abstruct class DrinkTemplate{
         final fun boilWater()
         abstruct fun getPot()
         abstruct fun putSource()
         final fun makeDrink()
    }
    模板方法是指：定义一个算法流程的骨架，把一些可变节点延迟到具体的子类中去执行，符合OCP和SRP。


## 观察者模式 
定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
观察者模式（Observer）又称发布-订阅模式（Publish-Subscribe：Pub/Sub）。它是一种通知机制，让发送通知的一方（被观察方）和接收通知的一方（观察者）能彼此分离，互不影响。
要理解观察者模式，我们还是看例子。

