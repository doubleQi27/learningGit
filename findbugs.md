代码分析工具findbugs和spotbugs

一、findbugs
1、简介：开源静态代码分析工具，检查类或者jar文件，将字节码（.class文件）
   与一组缺陷模式进行对比以发现可能的问题(先对编译后的class进行扫描，然后
   进行对比)，寻找出真正的缺陷和潜在的性能问题。在开发阶段和维护阶段都可使用。

2、能检测的类型包括：
   ①正确性：如强制类型转换；
   ②标准：某个类实现了equals方法而未实现hashCode方法；
   ③多线程检查：主要检查多线程同步中可能出现的问题；
   ④性能：潜在的性能委托问题；
   ⑤安全：可能导致安全的相关问题；
   ……

3、findbugs使用
   可对选定的project、module或文件进行分析，前提是该project、module中的所有
   .java文件均为已编译，有对应的.class文件。

4、9种bug类型：
   ① Bad practice
   不好的实践。比如不规范的类、方法、变量的命名；调用有返回值的方法时没有正确
   使用返回值等。
   ② Dodgy code
   可疑的代码。比如使用switch/case语句，但没有使用default；数据计算丢失精度等。
   ③ Malicious code vulnerability
   恶意的代码漏洞，如果代码公开，可能导致恶意的修改和攻击。比如该使用final修改的常量
   没有使用final，导致该常量可能被修改；该使用protected修饰的，却使用public修饰，这
   不利于对成员的保护.
   ④ Correctness
   正确性问题，不正确使用会导致报错。比如该判断空指针的地方没有判空，这会导致空指针异常；
   构造函数中使用了没有复制的变量，如果使用时对该变量有判空，该语句不会执行，而如果没有判空，
   则会报空指针异常。
   ⑥ Perfoamance
   性能问题。比如定义了某些变量却从未被读取过；定义的变量从未被使用过等，在执行过程中会占用空间和时间，
   建议删掉；内部类应该加上static修饰符；可以采用更高效的函数来实现功能等。

   ⑥ Multithreaded correctness
   多线程正确性，在多线程环境下的线程安全问题。比如volatile的不正确使用；该使用syncronized关键字的地方
   没有使用等。

   ⑦ Experimental
   实验。

   ⑧ Security
   安全性能问题。要扫描到安全方面的bug，需要在设置>FindBugs-IDEA >General > Plugins中添加Find Security Bugs插件。这里主要聚焦于安全方面的问题，比如向sdcard等存储卡中写数据，其中可能包含私人信息；写入sdcard等存储器数据容易被其它程序读取；使用的加密算法不够安全等。（此条，for Android Studio）

   ⑨ Internationalization(I18N)
   国际化问题。比如字符串与字节相互转换时的字符集问题，有些地方需要显示指定字符集，因为不同平台或语言使用的字符集有差异。

5、bug的严重级别
   ① Of Concren ：建议，如果遵循能更好的完善代码；
   ② Throubling ：不好的，可能会引发不良后果；
   ③ Scary      ：严重问题，在某种情况下一定会出现问题；
   ④ Scarist    ：非常严重，已经影响到当前程序功能。 

6、学习链接：https://www.cnblogs.com/andy-songwei/p/11820564.html。

二、spotbugs
1、简介：spotbugs可视为findbugs的加强版，是findbugs的继任者。

2、检测类型
① Bad practice
不佳实践，常见代码错误，用于静态代码检查时进行缺陷模式匹配(如重写equals但没重写
hashCode，或相反情况等)。

② Correctness
可能导致错误的代码，如空指针引用、无限循环。

③ Experimental
  实验性。

④ internationalization
国际化相关问题，如字符串转换等。

⑤ Malicious code vulnerability
可能受到恶意攻击，如访问权限修饰符（protected->public）、修改权限等（final）。

⑥ Multithreaded correctness
多线程的正确性（线程同步、线程调度等问题）。

⑦BogusMutilthreaded correctness
多线程的正确性（线程同步、线程调度等问题）。

⑧ Perfoamance
运行时性能问题，如findbugs的描述。

⑨ Security
安全性问题，如HTTP、SQL、DB等。

⑩Dodgy code
导致自身错误的代码（如未确认强制转换、冗余空值检查等）。