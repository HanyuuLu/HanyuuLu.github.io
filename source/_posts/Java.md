---
title: Java 复习提纲
date: 2019-01-10 00:00:00
tags: 
	- Java
categories:
	- review
---
* 垃圾回收 `System.gc()`
## Chapter 01
* 数据类型

类型|容量（bit）|范围|包装器
:-|:-|:-|:-
boolean|1|true\false|Boolean
char|16|Unicode|Character
byte|8|$[-128,127]$|Byte
short|16|$[-2^{15},2^{15}-1]$|Short
int|32|$[-2^{31},2^{31}-1]$|Integer
Long|64|$[-2^{63},2^{63}-1]$|Long
float|32|$3.4*10^{38}$|Long
double|64|$1.7*10^{308}$|Double
void|-|-|Void
* 自动类型转换
  
  * byte,short,char—> int —> long—> float —> double
* 数据类型转换`int a = (int)3.14159`
* Package
* Import
* Class
* Field
* Method
* Object
* Constract and Initialization
* Access Control
* Java修饰符
  * 访问控制修饰符 : default, public , protected, private
  * 非访问控制修饰符 : final, abstract, static, synchronized
* 继承
  * 在Java中，一个类可以由其他类派生。如果你要创建一个类，而且已经存在一个类具有你所需要的属性或方法，那么你可以将新创建的类继承该类。
  * 利用继承的方法，可以重用已存在类的方法和属性，而不用重写这些代码。被继承的类称为超类（super class），派生类称为子类（subclass）。
  * Java 源程序与编译型运行区别
  ![](https://www.runoob.com/wp-content/uploads/2013/12/ZSSDMld.png)
  * 一个类可以包含以下类型变量：
    * 局部变量：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
      * 局部变量声明在方法、构造方法或者语句块中；
        * 局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁；
        * 访问修饰符不能用于局部变量；
        * 局部变量只在声明它的方法、构造方法或者语句块中可见；
        * 局部变量是在栈上分配的。
        * 局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。
    * 成员变量：成员变量是定义在类中，方法体之外的变量。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
    * 类变量：类变量也声明在类中，方法体之外，但必须声明为static类型。
      * 类变量也称为静态变量，在类中以static关键字声明，但必须在方法构造方法和语句块之外。
      * 无论一个类创建了多少个对象，类只拥有类变量的一份拷贝。
      * 静态变量除了被声明为常量外很少使用。常量是指声明为public/private，final和static类型的变量。常量初始化后不可改变。
      * 静态变量储存在静态存储区。经常被声明为常量，很少单独使用static声明变量。
      * 静态变量在第一次被访问时创建，在程序结束时销毁。
      * 与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为public类型。
      * 默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化。
      * 静态变量可以通过：ClassName.VariableName的方式访问。
      * 类变量被声明为public static final类型时，类变量名称一般建议使用大写字母。如果静态变量不是public和final类型，其命名方式与实例变量以及局部变量的命名方式一致。
    * 实例变量
      * 实例变量声明在一个类中，但在方法、构造方法和语句块之外；
      * 当一个对象被实例化之后，每个实例变量的值就跟着确定；
      * 实例变量在对象创建的时候创建，在对象被销毁的时候销毁；
      * 实例变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息；
      * 实例变量可以声明在使用前或者使用后；
      * 访问修饰符可以修饰实例变量；
      * 实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见；
      * 实例变量具有默认值。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；
      * 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName。
    * 一个类可以拥有多个方法，在上面的例子中：barking()、hungry()和sleeping()都是Dog类的方法。 
  * 源文件声明规则
    * 一个源文件中只能有一个public类
    * 一个源文件可以有多个非public类
    * 源文件的名称应该和public类的类名保持一致。例如：源文件中public类的类名是Employee，那么源文件应该命名为Employee.java。
    * 如果一个类定义在某个包中，那么package语句应该在源文件的首行。
    * 如果源文件包含import语句，那么应该放在package语句和类定义之间。如果没有package语句，那么import语句应该在源文件中最前面。
    * import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。

* 接口
  * 在Java中，接口可理解为对象间相互通信的协议。接口在继承中扮演着很重要的角色。
  * 接口只定义派生要用到的方法，但是方法的具体实现完全取决于派生类。
* 继承关键字 
  * 继承可以使用 extends 和 implements 这两个关键字来实现继承，而且所有的类都是继承于 java.lang.Object，当一个类没有继承的两个关键字，则默认继承object（这个类在 java.lang 包中，所以不需要 import）祖先类。
  * extends关键字
    * 在 Java 中，类的继承是单一继承，也就是说，一个子类只能拥有一个父类，所以 extends 只能继承一个类。
  * implements关键字
    * 使用 implements 关键字可以变相的使java具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口（接口跟接口之间采用逗号分隔）。
  * super 与 this 关键字
    * super关键字：我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。
    * this关键字：指向自己的引用。
  * final关键字
      * final 关键字声明类可以把类定义为不能继承的，即最终类；或者用于修饰方法，该方法不能被子类重写：
      * 声明类：
        * final class 类名 {//类体}
      * 声明方法：
        * 修饰符(public/private/default/protected) final 返回值类型 方法名(){//方法体}
  * 构造器
    * 子类是不继承父类的构造器（构造方法或者构造函数）的，它只是调用（隐式或显式）。如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 super 关键字调用父类的构造器并配以适当的参数列表。
    * 如果父类构造器没有参数，则在子类的构造器中不需要使用 super 关键字调用父类构造器，系统会自动调用父类的无参构造器。 
  ![](https://www.runoob.com/wp-content/uploads/2013/12/types_of_inheritance.png)
* Java 抽象类
  * 在面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。 
  * 抽象类除了不能实例化对象之外，类的其它功能依然存在，成员变量、成员方法和构造方法的访问方式和普通类一样。
  * 由于抽象类不能实例化对象，所以抽象类必须被继承，才能被使用。也是因为这个原因，通常在设计阶段决定要不要设计抽象类。
  * 父类包含了子类集合的常见的方法，但是由于父类本身是抽象的，所以不能使用这些方法。
  * 在Java中抽象类表示的是一种继承关系，一个类只能继承一个抽象类，而一个类却可以实现多个接口。
  * 抽象方法
      * 如果你想设计这样一个类，该类包含一个特别的成员方法，该方法的具体实现由它的子类确定，那么你可以在父类中声明该方法为抽象方法。
      * Abstract关键字同样可以用来声明抽象方法，抽象方法只包含一个方法名，而没有方法体。
      * 抽象方法没有定义，方法名后面直接跟一个分号，而不是花括号。
    * 声明抽象方法会造成以下两个结果：
      * 如果一个类包含抽象方法，那么该类必须是抽象类。
      * 任何子类必须重写父类的抽象方法，或者声明自身为抽象类。
      * 继承抽象方法的子类必须重写该方法。否则，该子类也必须声明为抽象类。最终，必须有子类实现该抽象方法，否则，从最初的父类到最终的子类都不能用来实例化对象。
  1. 抽象类不能被实例化(初学者很容易犯的错)，如果被实例化，就会报错，编译无法通过。只有抽象类的非抽象子类可以创建对象。
  2. 抽象类中不一定包含抽象方法，但是有抽象方法的类必定是抽象类。
  3. 抽象类中的抽象方法只是声明，不包含方法体，就是不给出方法的具体实现也就是方法的具体功能。
  4. 构造方法，类方法（用static修饰的方法）不能声明为抽象方法。
  5. 抽象类的子类必须给出抽象类中的抽象方法的具体实现，除非该子类也是抽象类。

Java 接口
 *  接口（英文：Interface），在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
 *  接口并不是类，编写接口的方式和类很相似，但是它们属于不同的概念。类描述对象的属性和方法。接口则包含类要实现的方法。
 *  除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。
 *  接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。另外，在 Java 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。
 *  接口与类相似点：
 *  一个接口可以有多个方法。
    *  接口文件保存在 .java 结尾的文件中，文件名使用接口名。
    *  接口的字节码文件保存在 .class 结尾的文件中。
    *  接口相应的字节码文件必须在与包名称相匹配的目录结构中。
 *  接口与类的区别：
    *  接口不能用于实例化对象。
    *  接口没有构造方法。
    *  接口中所有的方法必须是抽象方法。
    *  接口不能包含成员变量，除了 static 和 final 变量。
    *  接口不是被类继承了，而是要被类实现。
    *  接口支持多继承。
 *  接口特性
    *  接口中每一个方法也是隐式抽象的,接口中的方法会被隐式的指定为 public abstract（只能是 public abstract，其他修饰符都会报错）。
    *  接口中可以含有变量，但是接口中的变量会被隐式的指定为 public static final 变量（并且只能是 public，用 private 修饰会报编译错误）。
    *  接口中的方法是不能在接口中实现的，只能由实现接口的类来实现接口中的方法。
  * 抽象类和接口的区别
  1. 抽象类中的方法可以有方法体，就是能实现方法的具体功能，但是接口中的方法不行。
  2. 抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是 public static final 类型的。
  3. 接口中不能含有静态代码块以及静态方法(用 static 修饰的方法)，而抽象类是可以有静态代码块和静态方法。
  4. 一个类只能继承一个抽象类，而一个类却可以实现多个接口。
## Chapter 02 OOP
* Object
* Class
* Abstraction
* Inheritance
* Polymorphism
* `finalilze();`
* `Abstraction`
* `interface`
* `implements`
## Chapter 03 Exception
* 检查性异常：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
* 运行时异常： 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
* 错误： 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。
* Exception 类的层次
  * 所有的异常类是从 java.lang.Exception 类继承的子类。 
  * Exception 类是 Throwable 类的子类。除了Exception类外，Throwable还有一个子类Error 。 
  * Java 程序通常不捕获错误。错误一般发生在严重故障时，它们在Java程序处理的范畴之外。 
  * Error 用来指示运行时环境发生的错误。 
  ![](https://www.runoob.com/wp-content/uploads/2013/12/12-130Q1234I6223.jpg)
## Chapter 04 Java I/O
``` java
	File file = new File("filePath/fileName");
	System.out.println(file.exists());
	System.out.println(file.isFile());
	File[] files = file.listFlies(filter);
	System.out.println(files.length);
	Arrays.sort(files,comparator);
```
* Stream
  * Byte stream
    * java.io.InputStream
      * `int read()	//read a byte`
    * java.io.OutputStream
      * `void write(int b)`
      * `void write(byte[] b)`
	* FileInputStream, FileOutputStream
	* PipedInputStream, PipedOutputStream
	* ByteArrayInputStream, ByteArrayOutputStream
	* BufferedInputStream, BufferedOutputStream
	* ObjectInputStreamm ObjectOutputStream
  * Character stream
    * java.io.Reader
      * `int read()	//read a char`
    * java.io.Writer
      * `void write(int b)`
      * `void write(char[] c)`
    * FileReader, FileWriter
    * PipedReader, PipedWriter
    * BufferedReader, BufferedWriter
    * InpputStreamReader, OutputStreamWriter
  * Bridge
    * InputStreamReader
    * OutputStreamWriter
* FileInputStream
  * ``` java
  	FileInputStream fis = FileInputStream(file);
	  int  res = fis.read();	//IOException
	  fis.available();	//是否可用
	  fis.close();
	  ```
	```
	
	```
* FileOutputStream
  * ``` java
  	file.createNewFile()
	fos.write(2);
	fos.write('a');
		```
	```
	
	```
* FileReader
* FileWriter
  * ```java
    FileWriter w = new FileWriter(new File("a.txt"),true);
	w.write("Hanyuu".toCharArray());
	w.flush();
  	```
* InputStreamReader
* OutputStreamReader
* PrintStream
* DataInputStream, DataOutputStream
* PrintWriter
* Scanner
  
  * java.util.Scanner
* BufferedInputStream, BufferedOutputStream
  * `BufferedInputStream bufferedInput = new BufferedInputStream(new FileInputStream(filename));`
  * ``` java
	  public void testBufferedInput() {
    try {
        /**
         * 建立输入流 BufferedInputStream, 缓冲区大小为8
         * buffer.txt内容为
         * abcdefghij
         */
        InputStream in = new BufferedInputStream(new FileInputStream(new File("buff.txt")), 8);
        /*从字节流中读取5个字节*/
        byte [] tmp = new byte[5];
        in.read(tmp, 0, 5);
        System.out.println("字节流的前5个字节为: " + new String(tmp));
        /*标记测试*/
        in.mark(6);
        /*读取5个字节*/
        in.read(tmp, 0, 5);
        System.out.println("字节流中第6到10个字节为: " +  new String(tmp));
        /*reset*/
        in.reset();
        System.out.printf("reset后读取的第一个字节为: %c" , in.read());
    } catch (Exception e) {
        e.printStackTrace();
    }
	  }
	  ```
  ```
  * InputStreamReader
  * OutputStreamReader
  * System.in
  * System.out
  * System.err
  * DataOutputStream
  * DataInputStream
  ```

## Chapter 05 Collection
* Arrays
  * java.util.Arrays
  * `Arrays.sort()`
  * `Arrays.fill(String[],String)`
  * `Array.hashCode(String)`
* Collection
  * `Set<E> //non-repeat`
    * `SortedSet<E>`
  * `List<E>`
  * `Quene<E>`
* Map
  * `HashMap<K,V>`
    ``` java
    HashMap<Integer,String> map = new HashMap<Integer,String>();
    map.put(0,"Hanyuu");
    String name = map.get(0);
    map.remove(0);
    Set keySet = map.keySet();
    Collection valueSet = map.values();
    Set entrySet = map.entrySet();
    ```
  * `SortedMap<K,V>`
* Iterator
* Seqential and Linear
* Use Array as Backend
* Varible Length
* Methods
  ``` java
  add(Object)
  add(int index,Object)
  remove(Object)
  get(int)
  set(int)
  indexOf(Objects)
  clear()
  Size()
  toArray()
  ```
* ArrayList
  * `ArrayList<String> list = new ArrayList<String>();`
  * `ArrayList<String> list = new ArrayList<String>(100);`
  * `list.ensureCapacity(1000)`
* LinkedList
  ``` java
  LinkdList<String> list = new LinkedList<String>();
  list.add("Hanyuu");
  Iterator<String> iterator = list.iterator();
  while(iterator.hasNext()){
    System.out.println(iterator.next());
  }
  ```
## Chapter 07 UI
  * java.jwt
  * javax.swing
  * `JFrame frame = new JFrame(String //title);`
    * `frame.getContentPane().add(BorderLayout.EAST,button);`
    * `frame.setSize(300,400);`
    * `frame.setVisible(true);`
    * `frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);`
    * `Container cp = frame.getContentPane();`
    * `cp.setLayout(new FloatLayout());`
    * `cp.add(new JPanel)();`
  * `JButton button = new JButton("Okey");`
  * `JComboBox`
  * `JCheckBox`
  * `JPanel`
    
    * `pnel.setLayout(new FLowLayout(FlowLayout.LEFT));`
  * `JPanel //container`
    * `JPanle = new JPanel();`
    * `panel.add(new JTextField("Hanyuu"));`
  * `JSplistPane`
  * `JScrollPane`
  * `Graphics`
  * `JLabel`
      * `getText();`
      * `setText();`
      * `setIcon();`
  * `JTextField`
  * `JCheckBox`
  * `JTextArea`
  * `JRadioButton`
  * 事件侦听
    * ActionLinstener
      ``` java
      public class Action implements ActionListener{
        public void actionPerformed(ActionEvent event){
          //TODO...
        }
      }
      JButton exp = new JButton();
      exp.addActionLintener(new Action());
      ```
## Chapter 08 Multi-thread
 1. `runnable` interface
    ``` java
    public class Task implements Runnable{
      public void run(){
        //TODO...
      }
    }
    public static void main(String[] args){
      Thread thread = new Thread(new Task(),"Thread name");
      thread.start();
    }
    ```
 2. Thread
   ``` java
   public class HanyuuThread extends Thread{
     public void run(){
       //TODO...
     }
   }
   public static void main(String[] args){
     HanyuuThread hanyuu = new HanyuuThread();
     hanyuu.setName("Hanyuu");
     hanyuu.statrt();
   }
   ```
   * `Thread.sleep(int)`
   * `String  Thread.currentThread().getName();`
   * `join`
   * `interrupt()`
   * `yield()`
  > yield意味着放手，放弃，投降。一个调用yield()方法的线程告 诉虚拟机它乐意让其他线程占用自己的位置。这表明该线程没 有在做一些紧急的事情。注意，这仅是一个暗示，并不能保证 不会产生任何影响。 \
    Yield告诉当前正在执行的线程把运行机会交给线程池中拥有相 同优先级的线程。\
    Yield不能保证使得当前正在运行的线程迅速转换到可运行的状态。\
    它仅能使一个线程从运行状态转到可运行状态，而不是等待或阻塞状态
   * `notify()`/`notifyAll()`
     * notifyAll() wakes all waiting thread, thus, all waiting thread turn to Ready
     * notify() only wakes one of waiting thread, others remain blocked
   * `sleep()`
     * java.lang.Thread`
   * `wait()`
     * `java.lang.Object`
     * Each object has a wait method, inherited from java.lang.Object
     * wait() method ask current thread to give up exclusive control
     * wait() method give other thread a chance to visit the object
     * wait() / wait(long timeout)
   *  wait() / notifyAll() / notify()
      *  The object must be locked before visit these methods
      *  They can be used in synchronized method of an object
      *  Or obj.wait() / obj.notifyAll() / obj.notify() in synchorized(obj){…}
      *  Otherwise: java.lang.IllegalMonitorStateException
   * `synchronized`
    ``` java
    public class Hanyuu{
      public synchronized void onlyOne(){
        //TODO...
      }
      public synchronized void threadSafty(){
        //TODO...
      }
    }
    ```
## Chapter 09 Java & XML
## Chapter 10 JDBC
* Record
* Field
* Table
* Entity
* Relations
* DataBase
* Primary key
* Foreign key
* Select
  * `SELECT nameA FROM tableA`
  * `SELECT nameA FROM tableA WHERE name > 2`
  * `SELECT nameA FROM tableA WHERE (name > 2 AND name < 10) OR name >300`
  * `SELECT * FROM tableA WHERE name IN ('Hanyuu','Inari')`
  * `SELECT * FROM tableA WHERE date BETWEEN 'Jan-01-2019' AND 'Jan-02-2019'`
  * `SELECT * FROM tableA WHERE name LIKE '%an%'`
  * `SELECT * FROM tableA ORDER BY name DESC/ASC`
  * `SELECT COUNT(DISTINCT name) FROM tableA`
* Insert
  
  * `INSERT INTO tableA (name,Date) VALUES ('Inari','Jan-01-2019')`
* Retireval
* Update
  
  * `UPDATE tableA SET date = 'Jan-01-2019' WEHERE name = 'Hanyuu'`
* Delete
  * `DELETE FROM tableA WHERE name = 'Inari'`
  > 你知道为什么SQL语句大家都选择大写嘛？（hhh）
  ``` java
  package SQL;
  import java.sql.*;
  import javax.sql.*;
  public class SQL {
  	public static void main(String[] args) {
  //		ResultSet rs;
  //		Statement statement;
  //		Connection connection;
  		Connection connection = null;
  		Statement statement = null;
  		ResultSet rs = null;
  		try {
        //注册驱动程序
  			Class.forName("com.mysql.cj.jdbc.Driver");
        //创建JDBC连接
  			String dbURL = "jdbc:mysql://localhost:3306/Hanyuu?user=Hanyuu&password=Hanyuu&useSSL=false&serverTimezone=GMT";
  			connection = DriverManager.getConnection(dbURL);
        //创建statement
  			String sqlQuery = "SELECT DISTINCT bookname FROM bookstore";
  			statement = connection.createStatement();
  			rs = statement.executeQuery(sqlQuery);
  			while (rs.next()) {
  				System.out.println(rs.getString("bookname"));
  			}
  			String nsqlQuery = "SELECT * FROM bookstore";
  			rs = statement.executeQuery(nsqlQuery);
        //执行查询语句
  			ResultSetMetaData rsmd = rs.getMetaData();
  			for (int i = 1; i <= rsmd.getColumnCount(); ++i) {
  				System.out.println(rsmd.getColumnName(i)+'\t');
  			}
  		} catch (ClassNotFoundException e) {
  			System.out.println("无驱动类");
  		} catch (SQLException e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				rs.close();
  				statement.close();
  				connection.close();
  			} catch (Exception e) {
  				e.printStackTrace();
  			}
  		}
  	}
  }
  ```
* 构建Prepared statement
  ``` java
  String sql = "INSERT INTO tableA (id,name,sorce) VALUES (?,?,?)";
  PreparedStatement s =connection.prepareStatement(sql);
  //add a record
  s.setInt(1,0000);
  s.setString(2,"Hanyuu");
  s.setInt(3,60);
  s.addBatch();
  s.clearParameters();
  s.executeBatch();
  s.cleatBatch();
  ```
## Chapter 11 Java network Programming
* IP address
* IPv4
* IPv6
* IP
* Host name
* Domain Name
* DNS
``` java
try{
  //get InetAddress
  InetAddress iAddress = InetAddress.getLocalHost();
  //get local IP
  String IP = iAddress.getHostAddress().toString();
  //get local host name
  String hostName = iAddress.getHostName().toString();
  System.out.println("IP address"+IP);
  System.out.println("Host name"+hostName);
}
catch (UnknownHostException e){
  e.printStackTrace();
}catch(Exception e){
  e.printStackTrace();
}
```
* 通过主机名获取所有IP
``` java
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.util.ArrayList;
import java.util.Iterator;

public class getLocal {
	public static void main(String[] args) {
		try {
			InetAddress ia=InetAddress.getLocalHost();
			System.out.println(ia.getHostAddress());
			System.out.println(ia.getAddress());
			System.out.println(ia.getHostName());

			String hostName=InetAddress.getLocalHost().getHostName();
			ArrayList<String> allIP=new ArrayList<String>();

			if (hostName.length()>0)
			{
				InetAddress[] addresses=InetAddress.getAllByName(hostName);
				for (int i=0;i<addresses.length;i++){
					allIP.add(addresses[i].getHostAddress().toString());
				}
			}
			for (Iterator iter=allIP.iterator();((Iterator) iter).hasNext();){
				System.out.println(ier.next().toString());
			}
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}

	}
}

```
* Constructor localhost InetAddress
``` java
InetAddress addr = InetAddress.getByName(null);
InetAddress addr = InetAddress.getByName("127.0.0.1");
InetAddress addr = InetAddress.getByName("localhost");
InetAddress addr = InetAddress.getLocalHost();
byte[] IP = {(byte)127,(byte)0,(byte)0,(byte)1};
```
* Web server
* FTP server
* Mail server
* Port 1-1024 is occupied by system
* client
``` java
//构建客户端socket
Socket client = new Socket("hanyuu.ml",8080);
//构建客户端socket（通过InetAddress）
InetAddress address = InetAddress.getByName("Hanyuu.ml");
Socket client = new Socket(address,8080);
InputStream is = socket.getInputStream();
OutputStream os = socket.getOutputStream();
is.close();
os.close();
```
Example
``` java
Socket socket = new Socket(ip,8008);
BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()),true);
out.print("Hi");
Thread.sleep(1000);
out.println("Hello");
socket.close();
```
* server
``` java
//创建服务端的ServerSocket监听客户请求
ServerSocket server = new ServerSocket(8080);
//客户端阻塞，等待连接
Socket serverSocket = server.accept();
```
Example
```java
try{
  ButteredReader in =new BufferedReader(new InputStreamReader(socket.getInputStream()));
  PrintWriter out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()),true);
  while(true){
    String str = in.readLine();
    if (str!=null && str.equals("Hi")) out.println("Hi, here is server.");
  }catch(Exception e){
    e.printStackTrace();
  }finally{
    socket.close();
    server.close();
  }
}
```
* TCP
* UDP
* read a web page
``` java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;

public class conn {
	public static void main(String[] args) {
		try{
			URL coseURL = new URL("http://cose.seu.edu.cn");
			URLConnection connection =coseURL.openConnection();
			BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
			String html = in.readLine();
			while(html!=null){
				System.out.println(html);
				html=in.readLine();
			}

		}catch (Exception e){

		}
	}
}
```
