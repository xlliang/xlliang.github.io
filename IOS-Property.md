#IOS-Property

##简介
属性（property）是Objective-C的一项特性，用于封装对象中的数据。这一特性可以令编译器自动编写,get,set方法和ivar，可以用下面公式表示@property = ivar + getter + setter

##Property限制语义
* 原子性： atomic/nonatomic
* 读写权限： readwrite/readonly
* 内存管理： assign/strong/copy/weak/unsafe_unretained
* 存取方法： getter=/setter=

默认设置

* 基本数据类型：atomic, readwrite, assign
* 对象类型：atomic, readwrite, strong

<mark>注意：从代码可读性以及可维护性上，建议关键词的顺序是：原子性、读写权限、内存管理语义、getter/getter。<mark>


###atomic/nonatomic
原子性是什么？atomic和nonatomic的区别是什么？atomic百分百安全吗？（本文的安全与否针对property,不是atomic本身）

* 原子性：保证同一个时间片，只能有一个操作被执行，执行过程不能被干扰
* atomic：原子性的，编译器会锁定setter和getter的操作，保证完整性。
* nonatomic：非原子性的，不保证完整性。
* 区别：atomic线程相对安全，数度慢；nonatomic线程不安全，速度快。但是犹豫IOS中锁定机制开销很大，故此我们开发中都用nonatomic，而macOS中是使用atomic。

例如：如果线程 A 调了 getter，与此同时线程 B 、线程 C 都调了 setter——那最后线程 A get 到的值，3种都有可能：可能是 B、C set 之前原始的值，也可能是 B set 的值，也可能是 C set 的值。同时，最终这个属性的值，可能是 B set 的值，也有可能是 C set 的值。


###readwrite/readonly
默认是readwrite。一般性原则：.h中用readonly对外只提供可读，.m的Extension中用readwrite对内提供读写。

###getter=/setter=
指定方法别名。例如：

.h中： @property (nonatomic, assign, getter=isTest) BOOL test;

.m中：- (BOOL) isTest {
    return self.on;
}
 

###内存管理

* strong：修饰对象，引用计数+1，强行置空可以销毁。
* copy：和strong类似。会拷贝副本。
* weak：不拥有对象。引用计数增加，对象销毁，属性自动设置为null,不会引起野指针异常。block,delegate,xib中常用。
* assin：用于基本类型，如NSIteger、CGFloat等，这些数值主要存在于栈中。
* unsafe_unretained：与weak类似，但是销毁时不自动清空，容易形成野指针。

* copy与strong：相同之处是都持有对象。不同之处是strong是多个指针指向同一个地址，而copy的是每次会在内存中复制一份对象，指针指向不同的地址。NSString、NSArray、NSDictionary等不可变对象用copy修饰，因为有可能传入一个可变的版本，此时能保证属性值不会受外界影响。

* 注意：若用strong修饰NSArray，当数组接收一个可变数组，可变数组若发生变化，被修饰的属性数组也会发生变化，也就是说属性值容易被篡改；若用copy修饰NSMutableArray，当试图修改属性数组里的值时，程序会崩溃，因为数组被复制成了一个不可变的版本。

##属性相关的操作
###@dynamic
属性的setter与getter方法由用户自己实现，不自动生成。如果没有生成，编译不会有报警告，运行时候会导致crash
###@synthesize
是语法糖，告诉编译器自动合成getter,setter方法。

以前会@synthesize firstName = _myFirstName; 这样的写法，给变量指定别名。但是后来，这一步也不用做了，爽吧！
