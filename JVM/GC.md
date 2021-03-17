### Garbage Collection（垃圾收集）

参考链接[CS-Note/Java](https://github.com/CyC2018/CS-Notes/blob/master/notes/Java%20%E8%99%9A%E6%8B%9F%E6%9C%BA.md#%E7%A8%8B%E5%BA%8F%E8%AE%A1%E6%95%B0%E5%99%A8)

#### 1.判断一个对象是否可以被回收

- **引用计数**算法

  对象被引用一次则加一，失去一个引用则减一。

  由于存在循环引用的问题，所以 JVM 不采用这种算法。

- **可达性分析**算法

  以 Java Roots 为起点进行搜索，可达的对象都是存活的，不可达的对象被回收。

  JVM 使用这种方法来判断对象是否可以被回收。

  包含以下内容：

  - 虚拟机栈中局部变量表中引用的对象
  - 本地方法栈中 JNI 中引用的对象
  - 方法区中类静态属性引用的对象
  - 方法区中的常量引用的对象

- 方法区的回收

  方法区主要存放**永久代对象**，在方法区进行回收的性价比不高。

  主要对**常量池的回收**和对**类的卸载**。

  类的卸载条件：

  - 该类所有的实例都已经被回收，此时堆中不存在该类的任何实例。
  - 加载该类的 [ClassLoader](https://zhuanlan.zhihu.com/p/51374915]) 已经被回收。
  - 该类对应的 Class 对象没有在任何地方被引用，也就无法在任何地方通过[反射](https://www.cnblogs.com/chanshuyi/p/head_first_of_reflection.html)访问该类方法。

  > **类加载器 ClassLoader**
  >
  > - BootstrapClassLoader（加载核心类，java.util.*、java.io.*、java.nio.*、java.lang.*）
  > - ExtensionClassLoader（加载扩展类，如 javax中的Swing） 
  > - AppClassLoader(面向用户，如 Main 类)。

  > **反射** Reflection
  >
  > - 获取 Class 对象实例 
  >
  >   ```java
  >   Class clz = Class.forName("com.zhenai.api.Apple");
  >   ```
  >
  > - 根据 Class 对象获取 Constructor 对象
  >
  >   ```java
  >   Constructor appleConstructor = clz.getConstructor();
  >   ```
  >
  > - 使用 Constructor对象的 newInstance 方法获取反射类对象
  >
  >   ```java
  >   Object appleObj = appleConstructor.newInstance();
  >   ```
  >
  > - 调用方法
  >
  >   - 获取方法的 Method 对象
  >
  >     ```java
  >     Method setPriceMethod = clz.getMethod("setPrice", int.class);
  >     ```
  >
  >   - 使用 invoke 方法调用方法
  >
  >     ```java
  >     setPriceMethod.invoke(appleObj, 14);
  >     ```

#### 2. 引用类型

判断对象是否可以被回收，都与**引用**有关系

new、SoftReference、WeakReference、PhantomReference；

- 强引用

  new 得到的

  ```java
  Object obj = new Object();
  ```

- 软引用

  被软引用关联的对象只有在**内存不够的时候**才会被回收。

  ```java
  Object obj = new Object();
  SoftReference<Object> sf = new SoftReference<Object>(obj);
  obj = null;  // 使对象只被软引用关联
  ```

- 弱引用

  被弱引用关联的对象一定会被回收，它只能存活到下一次垃圾回收发生之前。

  ```java
  Object obj = new Object();
  WeakReference<Object> wf = new WeakReference<Object>(obj);
  obj = null;
  ```

- 虚引用

  又称为**幽灵引用**或**幻影引用**。

  不会影响对象的生存时间，无法通过虚引用来访问对象。

  唯一目的：**对象被回收时会收到一个系统通知**。

  ```java
  Object obj = new Object();
  PhantomReference<Object> pf = new PhantomReference<Object>(obj, null);
  obj = null;
  ```

#### 3. 垃圾收集算法

- 标记-清除
- 标记-整理
- 复制
- 分代收集

#### 4. 垃圾收集器

