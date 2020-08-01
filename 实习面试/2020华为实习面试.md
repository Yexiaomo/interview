# 2020/7 华为实习面试回顾

## 华为面试问题汇总(技术面)

## Q1：讲讲排序算法都有哪些？各自都有什么优缺点？时间复杂度怎么样？稳定度？是内排还是外排？ ##


### 术语说明 ：   
 **稳定** ： 如果a原本在b前面，而a=b，排序之后a仍然在b的前面；    
 **不稳定** ：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；  
 **内排序**：所有排序操作都在内存中完成；  
 **外排序** ：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；      
  
### 算法总结： 

![算法总结](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzMwNDMxNjgtMTg2NzgxNzg2OS5wbmc?x-oss-process=image/format,png)  
### 排序分类：
![算法分类](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMyMDE3LmNuYmxvZ3MuY29tL2Jsb2cvODQ5NTg5LzIwMTcxMC84NDk1ODktMjAxNzEwMTUyMzMyMjA2MzctMTA1NTA4ODExOC5wbmc?x-oss-process=image/format,png)

### 比较和区别  

- 常见的快速排序、归并排序、堆排序、冒泡排序 等属于比较排序 。在排序的最终结果里，元素之间的次序依赖于它们之间的比较。每个数都必须和其他数进行比较，才能确定自己的位置 。

- 在冒泡排序之类的排序中，问题规模为n，又因为需要比较n次，所以平均时间复杂度为O(n²)。在归并排序、快速排序之类的排序中，问题规模通过分治法消减为logN次，所以时间复杂度平均O(nlogn)。

- 比较排序的优势是，适用于各种规模的数据，也不在乎数据的分布，都能进行排序。可以说，比较排序适用于一切需要排序的情况。

- 计数排序、基数排序、桶排序则属于非比较排序 。非比较排序是通过确定每个元素之前，应该有多少个元素来排序。针对数组arr，计算arr[i]之前有多少个元素，则唯一确定了arr[i]在排序后数组中的位置 。

- 非比较排序只要确定每个元素之前的已有的元素个数即可，所有一次遍历即可解决。算法时间复杂度O(n)。

- 非比较排序时间复杂度底，但由于非比较排序需要占用空间来确定唯一位置。所以对数据规模和数据分布有一定的要求。


---

# **Q2**：项目细节，项目开发遇到哪些问题，怎么解决的，收获是什么？ 
   
# **Q3**：手撕代码，完了会问有哪些注意的点？   

---
# **Q4**： 二叉树的遍历、删除、插入   #
### 二叉树的四种遍历方式： ###
- 二叉树的遍历（traversing binary tree）是指从根结点出发，按照某种次序依次访问二叉树中所有的结点，使得每个结点被访问依次且仅被访问一次。
- 四种遍历方式分别为：先序遍历、中序遍历、后序遍历、层序遍历。  
![遍历二叉树](https://img2018.cnblogs.com/blog/1542838/201907/1542838-20190722222821663-1408544995.png)

创建一个二叉树：  
声明节点**TreeNode**类，代码如下：
```java
public class TreeNode {
    public int data;
    public TreeNode leftChild;
    public TreeNode rightChild;
    public TreeNode(int data){
        this.data = data;
    }
}
```

创建二叉树：
```java  
/**
 * 构建二叉树
 * @param list   输入序列
 * @return
 */
public static TreeNode createBinaryTree(LinkedList<Integer> list){
    TreeNode node = null;
    if(list == null || list.isEmpty()){
        return null;
    }
    Integer data = list.removeFirst();
    if(data!=null){
        node = new TreeNode(data);
        node.leftChild = createBinaryTree(list);
        node.rightChild = createBinaryTree(list);
    }
    return node;
}
```
**先序遍历：  （根左右）**  
遍历顺序：按图中数字顺序
![](https://img2018.cnblogs.com/blog/1542838/201907/1542838-20190722224917877-2136323533.png)  
遍历结果：**ABDFECGHI**  
递归代码实现：
```java  
    /* 递归方法实现
     * 二叉树前序遍历   根-> 左-> 右
     * @param node    二叉树节点
     */
    public static void preOrderTraveral(TreeNode node){
        if(node == null){
            return;
        }
        System.out.print(node.data+" ");
        preOrderTraveral(node.leftChild);
        preOrderTraveral(node.rightChild);
    }
```
非递归实现：  
1. 首先申请一个新的栈，记为stack；  
2. 声明一个结点treeNode，让其指向node结点；  
3. 如果treeNode的不为空，将treeNode的值打印，并将treeNode入栈，然后让treeNode指向treeNode的右结点，  
4. 重复步骤3，直到treenode为空；    
5. 然后出栈，让treeNode指向treeNode的右孩子  
6. 重复步骤3，直到stack为空.    

非递归实现代码如下：（先序遍历）
```java  
        /**
        非递归
        */
        public static void preOrderTraveralWithStack(TreeNode node){
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode treeNode = node;
        while(treeNode!=null || !stack.isEmpty()){
            //迭代访问节点的左孩子，并入栈
            while(treeNode != null){
                System.out.print(treeNode.data+" ");
                stack.push(treeNode);
                treeNode = treeNode.leftChild;
            }
            //如果节点没有左孩子，则弹出栈顶节点，访问节点右孩子
            if(!stack.isEmpty()){
                treeNode = stack.pop();
                treeNode = treeNode.rightChild;
            }
        }
    }
```
---    
**中序遍历：（左根右）**    

遍历顺序：按图中数字顺序  
![](https://img2018.cnblogs.com/blog/1542838/201907/1542838-20190722225534764-1572433775.png)  
遍历结果：  **DBEFAGHCI**   
#
递归代码实现：  
```java  
    /**递归
     * 二叉树中序遍历   左-> 根-> 右
     * @param node   二叉树节点
     */
    public static void inOrderTraveral(TreeNode node){
        if(node == null){
            return;
        }
        inOrderTraveral(node.leftChild);
        System.out.print(node.data+" ");
        inOrderTraveral(node.rightChild);
    }  
```
非递归实现：  
1. 申请一个新栈，记为stack，申请一个变量cur，初始时令treeNode为头节点；  
2. 先把treeNode节点压入栈中，对以treeNode节点为头的整棵子树来说，依次把整棵树的左子树压入栈中，即不断令treeNode=treeNode.leftChild，然后重复步骤2；  
3. 不断重复步骤2，直到发现cur为空，此时从stack中弹出一个节点记为treeNode，打印node的值，并让treeNode= treeNode.right，然后继续重复步骤2；  
4. 当stack为空并且cur为空时结束。  

#
非递归代码实现：(中序遍历)
```java  
        public static void inOrderTraveralWithStack(TreeNode node){
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode treeNode = node;
        while(treeNode!=null || !stack.isEmpty()){
            while(treeNode != null){
                stack.push(treeNode);
                treeNode = treeNode.leftChild;
            }
            if(!stack.isEmpty()){
                treeNode = stack.pop();
                System.out.print(treeNode.data+" ");
                treeNode = treeNode.rightChild;
            }

        }
    }
```

**后序遍历：（左右根）**    

遍历顺序：按图中数字顺序  
![](https://img2018.cnblogs.com/blog/1542838/201907/1542838-20190722225822242-120610112.png)  
遍历结果：  **DEFBHGICA**   
#
代码实现：  
```java  
    /**
     * 二叉树后序遍历   左-> 右-> 根
     * @param node    二叉树节点
     */
    public static void postOrderTraveral(TreeNode node){
        if(node == null){
            return;
        }
        postOrderTraveral(node.leftChild);
        postOrderTraveral(node.rightChild);
        System.out.print(node.data+" ");
    }
```
后续非递归相对前2个较为复杂，我们需要一个标记来记录当前节点的上一个节点，了解就行，不需要必须掌握。具体实现如下：
```java  
public static void postOrderTraveralWithStack(TreeNode node){
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode treeNode = node;
    TreeNode lastVisit = null;   //标记每次遍历最后一次访问的节点
    while(treeNode!=null || !stack.isEmpty()){//节点不为空，结点入栈，并且指向下一个左孩子
        while(treeNode!=null){
            stack.push(treeNode);
            treeNode = treeNode.leftChild;
        }
        //栈不为空
        if(!stack.isEmpty()){
            //出栈
            treeNode = stack.pop();
            /**
             * 这块就是判断treeNode是否有右孩子，
             * 如果没有输出treeNode.data，让lastVisit指向treeNode，并让treeNode为空
             * 如果有右孩子，将当前节点继续入栈，treeNode指向它的右孩子,继续重复循环
             */
            if(treeNode.rightChild == null || treeNode.rightChild == lastVisit) {
                System.out.print(treeNode.data + " ");
                lastVisit = treeNode;
                treeNode  = null;
            }else{
                stack.push(treeNode);
                treeNode = treeNode.rightChild;
            }

        }

    }
}
```

**层次遍历：**  
  
1. 首先申请一个新的队列，记为queue  
2. 将头结点head压入queue中  
3. 每次从queue中出队，记为node，然后打印node值，如果node左孩子不为空，则将左孩子入队；如果node的右孩子不为空，则将右孩子入队  
4. 重复步骤3，直到queue为空  
#
代码实现如下：
```java
        public static void levelOrder(TreeNode root){
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            root = queue.pop();
            System.out.print(root.data+" ");
            if(root.leftChild!=null) queue.add(root.leftChild);
            if(root.rightChild!=null) queue.add(root.rightChild);
        }
    }
```  

# **Q5**： 异常分类与处理 #

### 一、异常的分类：
![](https://img-blog.csdn.net/20180920165502957?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21pY2hhZWxnbw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上图简单的展示了常见异常的结构图：  
**1. 所有的异常都是从Throwable继承而来的，它是所有异常的共同祖先。**  
  **2. Throwable有两个子类，Error和Exception。**  

- 其中**Error**是错误，对于所有的编译时期的错误以及系统错误都是通过 Error 抛出的。这些错误表示故障发生于虚拟机自身、或者发生在虚拟机试图执行应用时，如Java虚拟机运行错误（**Virtual MachineError**）、类定义错误（**NoClassDefFoundError**）等。这些错误是不可查的，因为它们在应用程序的控制和处理能力之外，而且绝大多数是程序运行时不允许出现的状况。对于设计合理的应用程序来说，即使确实发生了错误，本质上也不应该试图去处理它所引起的异常状况。在 Java中，错误通过Error的子类描述。   

-  **Exception**，是另外一个非常重要的异常子类。它规定的异常是程序本身可以处理的异常。异常和错误的区别是，异常是可以被处理的，而错误是没法处理的。   
   
**3. Checked Exception**  

- 可检查的异常，这是编码时常用的，所有**checked exception**都是需要在代码中处理的。它们的发生是可以预测的，正常的一种情况，可以合理的处理。比如**IOException**，或者一些自定义的异常。除了RuntimeException及其子类以外，都是checked exception。  

**4. Unchecked Exception**  


- **RuntimeException**及其子类都是unchecked exception。比如空指针异常(NullPointerException)，除数为0的算数异常**ArithmeticException**等等，这种异常是运行时发生，无法预先捕捉处理的。Error   也是**unchecked exception**，也是无法预先处理的。

#
### 二、异常的处理  
**代码中的异常处理其实是对可检查异常的处理。**  
#
#
**1. 通过try...catch语句块来处理：**  
e.g.
> 
    
    try
    {
       // 程序代码
    }catch(ExceptionName e1)
    {
       //Catch 块
    }
#
Catch 语句包含要捕获异常类型的声明。当保护代码块中发生一个异常时，try 后面的 catch 块就会被检查。

如果发生的异常包含在 catch 块中，异常会被传递到该 catch 块，这和传递一个参数到方法是一样。  
#
**2. 另外，也可以在具体位置不处理，直接抛出，通过throws/throw到上层再进行处理。**  
 
具体的，如果一个方法没有捕获到一个检查性异常，那么该方法必须使用 throws 关键字来声明。throws 关键字放在方法签名的尾部。也可以使用 throw 关键字抛出一个异常，无论它是新实例化的还是刚捕获到的。  
#
下面方法的声明抛出一个 RemoteException 异常：  

>
    import java.io.*;
    public class className
    {
      public void deposit(double amount) throws RemoteException
      {
        // Method implementation
        throw new RemoteException();
      }
      //Remainder of class definition
    }

#
**3. finally关键字**  
finally 关键字用来创建在 try 代码块后面执行的代码块。

无论是否发生异常，finally 代码块中的代码总会被执行。

在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

finally 代码块出现在 catch 代码块最后，语法如下：  
>  
    try{
      // 程序代码
    }catch(异常类型1 异常的变量名1){
      // 程序代码
    }catch(异常类型2 异常的变量名2){
      // 程序代码
    }finally{
      // 程序代码
    }

#
# **Q6**： 讲一讲大文件(很大很大那种, 不跨平台) 的读写流程,有哪些注意点？   
#
BufferedReader类读写

>
    File file = new File(filepath);   
    BufferedInputStream fis = new BufferedInputStream(new FileInputStream(file));    
    BufferedReader reader = new BufferedReader(new InputStreamReader(fis,"utf-8"),5*1024*1024);// 用5M的缓冲读取文本文件  
    String line = "";
    while((line = reader.readLine()) != null){
    //TODO: write your business
    }
#
# Q7： set与Map以及HashSet和HashMap的联系。 

## Set接口 extends Collection接口    
#
**Set接口的特点:**   

    1. 继承Collection接口
    2. 无序
    3. 存储的是对象的引用，不允许存储重复的元素  
    4. 没有索引,没有带索引的方法,也不能使用普通的for循环遍历
    
    
#
## Map<k,v>集合
**Map集合的特点:**
> 
    1.Map集合是一个双列集合,一个元素包含两个值(一个key,一个value)
    2.Map集合中的元素,key和value的数据类型可以相同,也可以不同
    3.Map集合中的元素,key是不允许重复的,value是可以重复的
    4.Map集合中的元素,key和value是一一对应

## HashSet集合 implements Set接口  
**HashSet特点:**  
>
    1.不允许存储重复的元素  
    2.没有索引,没有带索引的方法,也不能使用普通的for循环遍历  
    3.是一个无序的集合,存储元素和取出元素的顺序有可能不一致  
    4.底层是一个哈希表结构(查询的速度非常的快)  

## HashMap<k,v>集合 implements Map<k,v>接口 
**HashMap集合的特点:**
>
    1.HashMap集合底层是哈希表:查询的速度特别的快
        JDK1.8之前:数组+单向链表
        JDK1.8之后:数组+单向链表|红黑树(链表的长度超过8):提高查询的速度
    2.hashMap集合是一个无序的集合,存储元素和取出元素的顺序有可能不一致  

## LinkedHashMap<k,v>集合 extends HashMap<k,v>集合
**LinkedHashMap的特点:**
>   
    1.LinkedHashMap集合底层是哈希表+链表(保证迭代的顺序)
    2.LinkedHashMap集合是一个有序的集合,存储元素和取出元素的顺序是一致的  

## HashSet和HashMap的区别

    (1)HashMap 
                               
        a.实现了Map接口                         
        b.存储键值对                            
        c.调用put()向map中添加元素             
        d.HashMap使用键（Key）计算Hashcode    
        e.HashMap相对于HashSet较快，因为它是使用唯一的键获取对象     
    
    (2)HashSet

        a.实现Set接口
        b.仅存储对象
        c.调用add（）方法向Set中添加元素
        d.HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false
        e.HashSet较HashMap来说比较慢

# Q8： 常见的linux操作命令
# Linux常用操作命令
1.ls：能够列出当前目录下的所有内容。ls 命令的执行方式为：
> 
    # ls  [-选项]  [文件名或者目录名]
    -l：列出所要查看的内容详细信息，不但包括文件名，还包括文件大小，访问权限和所有者信息等
    -a：列出所有文件，包括隐藏文件
    -R：列出当前目录下的所有内容，并且将子目录的内容也一起列出来
    -d：仅列出目录本身，而不显示当前目录下的内容  

2.cd：改变当前路径。 其命令执行的方式为：
>
    # cd 路径
    .  ：代表当前目录
    .. ：代表上层目录
    ~  ：代表当前登录用户的宿主目录
    ~用户名：代表进入~后用户的宿主目录
    -  ：代表前一目录，，即进入当前目录之前操作的目录

3.cd：改变当前路径。 其命令执行的方式为：
>
        # cd 路径
        .  ：代表当前目录
        .. ：代表上层目录
        ~  ：代表当前登录用户的宿主目录
        ~用户名：代表进入~后用户的宿主目录
        -  ：代表前一目录，，即进入当前目录之前操作的目录
4.pwd：查看当前路径命令
>
    # pwd 


5.touch：改变文件创建时间及创建空文件命令
>
    # touch 文件名
    -a：修改atime时间，也就是文件内容被读取的时间。
    -m：修改mtime时间，也就是文件内容被修改的时间。
    
6.mkdir：创建目录命令
>
    # mkdir 目录名

7.rmkdir：删除空目录命令
>
    # rmkdir 目录名

8.rm：删除文件（目录）命令
>
    # rm  [-选项] 文件名或者目录名 
    -f：强制删除
    -r：删除目录
    -i：删除文件或者目录前是否询问

9.cp：复制命令
>
    # cp源文件名  目标路径

10.mv： 移动文件（目录）命令
>
    # mv要移动的文件 目标路径

11.cat： 显示文件内容命令
>
     # cat 文件名

12.head：  从头开始查看文件内容命令
>
    # head [-n] 文件名，n为数字，即设定能够查看的行数

13.tail： 从文件结尾开始显示文件内容，并且指定查看的行数
>
    # # tail [-n] 文件名 

14.more（less）： 分屏显示文件命令，对文件内容或者查询结果进行分屏显示
>
    # more 文件名 


# Q9： 冯诺依曼体系结构

**体系结构图：**  
![](https://bkimg.cdn.bcebos.com/pic/a8773912b31bb051973f1da5367adab44aede020?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5)  

# 内容：
美籍匈牙利数学家冯·诺伊曼于1946年提出存储程序原理，把程序本身当作数据来对待，程序和该程序处理的数据用同样的方式储存。 冯·诺依曼体系结构冯·诺伊曼理论的要点是：计算机的数制采用二进制；计算机应该按照程序顺序执行。人们把冯·诺伊曼的这个理论称为冯·诺伊曼体系结构。

# 组成：
此种结构由**运算器、控制器、存储器、输入设备和输出设备**五部分组成。其存储程序原理的基本点是程序驱动。
# 特点：
（1）以运算器为中心；

（2）在存储器中，指令和数据同等对待；

（3）存储器是按地址访问、按顺序线性编址的一堆结构，每个存储单元的位数都是固定的；

（4）指令是顺序执行的；

（5）指令由操作码和地址码组成；

（6）指令和数据均以二进制编码表示，采用二进制运算。




# Q10： 进程线程区别与联系
**进程：**  
是代码在数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位。 

**线程：**  
是进程的一个执行路径，一个进程中至少有一个线程，进程中的多个线程共享进程的资源。  
虽然系统是把资源分给进程，但是CPU很特殊，是被分配到线程的，所以线程是CPU分配的基本单位。

**区别：**  
进程：有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响。

线程：是一个进程中的不同执行路径。线程有自己的堆栈和局部变量，但线程之间没有单独的地址空间，一个线程死掉就等于整个进程死掉。

**联系：**  
一个进程中有多个线程，多个线程共享进程的堆和方法区资源，但是每个线程有自己的程序计数器和栈区域。

**总结：**  
1) 简而言之,一个程序至少有一个进程,一个进程至少有一个线程.

2) 线程的划分尺度小于进程，使得多线程程序的并发性高。

3) 另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。

4) 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。

5) 从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别

# Q11：多线程与并发

多线程是Java的特性，因为现在cpu都是多核多线程的，可以同时执行几个任务，为了提高jvm的执行效率，Java提供了这种多线程的机制，以增强数据处理效率。**多线程对应的是cpu，高并发对应的是访问请求**，可以用单线程处理所有访问请求，也可以用多线程同时处理访问请求。  
可以这样理解多线程是处理并发的一种编程方法，即并发需要多线程来实现。

# Q12：最短路径算法

# **Q13**： 斐波那契数列  

斐波那契数列指的是这样一个数列 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233，377，610，987，1597，2584，4181，6765，10946，17711，28657，46368……  

特别指出：第0项是0，第1项是第一个1。

这个数列从第三项开始，每一项都等于前两项之和。
斐波那契数列又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”。在数学上，斐波纳契数列以如下被以递归的方法定义：F(0)=0，F(1)=1, F(n)=F(n-1)+F(n-2)（n>=2，n∈N*）
```java 
    public class MainClass {
    public static long fibonacci(long number) {
        if ((number == 0) || (number == 1))
            return number;
        else
            return fibonacci(number - 1) + fibonacci(number - 2);
        }
        public static void main(String[] args) {
            for (int counter = 0; counter <= 10; counter++){
            System.out.printf("Fibonacci of %d is: %d\n",
            counter, fibonacci(counter));
        }
    }
    }
``` 
# **Q14**：TCP是如何保证可靠传输的 

TCP协议保证数据传输可靠性的方式主要有：                                       
- 校验和 
- 序列号
- 确认应答
- 超时重传
- 连接管理
- 流量控制
- 拥塞控制  

### 校验和
计算方式：在数据传输的过程中，将发送的数据段都当做一个16位的整数。将这些整数加起来。并且前面的进位不能丢弃，补在后面，最后取反，得到校验和。  
发送方：在发送数据之前计算检验和，并进行校验和的填充。  
接收方：收到数据后，对数据以同样的方式进行计算，求出校验和，与发送方的进行比对。  
![](https://img-blog.csdn.net/20180524102010286?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWNoZW54aWE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
注意：如果接收方比对校验和与发送方不一致，那么数据一定传输有误。但是如果接收方比对校验和与发送方一致，数据不一定传输成功。  
 
### 确认应答与序列号
序列号：TCP传输时将每个字节的数据都进行了编号，这就是序列号。  
确认应答：TCP传输的过程中，每次接收方收到数据后，都会对传输方进行确认应答。也就是发送ACK报文。这个ACK报文当中带有对应的确认序列号，告诉发送方，接收到了哪些数据，下一次的数据从哪里发。  
![](https://img-blog.csdn.net/20180524103121705?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWNoZW54aWE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
序列号的作用不仅仅是应答的作用，有了序列号能够将接收到的数据根据序列号排序，并且去掉重复序列号的数据。这也是TCP传输可靠性的保证之一。  

### 超时重传  
在进行TCP传输时，由于确认应答与序列号机制，也就是说发送方发送一部分数据后，都会等待接收方发送的ACK报文，并解析ACK报文，判断数据是否传输成功。如果发送方发送完数据后，迟迟没有等到接收方的ACK报文，这该怎么办呢？而没有收到ACK报文的原因可能是什么呢？

首先，发送方没有介绍到响应的ACK报文原因可能有两点：  

    1.数据在传输过程中由于网络原因等直接全体丢包，接收方根本没有接收到。
    2.接收方接收到了响应的数据，但是发送的ACK报文响应却由于网络原因丢包了。  

TCP在解决这个问题的时候引入了一个新的机制，叫做超时重传机制。简单理解就是发送方在发送完数据后等待一个时间，时间到达没有接收到ACK报文，那么对刚才发送的数据进行重新发送。如果是刚才第一个原因，接收方收到二次重发的数据后，便进行ACK应答。如果是第二个原因，接收方发现接收的数据已存在（判断存在的根据就是序列号，所以上面说序列号还有去除重复数据的作用），那么直接丢弃，仍旧发送ACK应答。  


那么发送方发送完毕后等待的时间是多少呢？如果这个等待的时间过长，那么会影响TCP传输的整体效率，如果等待时间过短，又会导致频繁的发送重复的包。如何权衡？

由于TCP传输时保证能够在任何环境下都有一个高性能的通信，因此这个最大超时时间（也就是等待的时间）是动态计算的。  

### 连接管理
连接管理就是三次握手与四次挥手的过程，在前面详细讲过这个过程，这里不再赘述。保证可靠的连接，是保证可靠性的前提。  

### 流量控制
接收端在接收到数据后，对其进行处理。如果发送端的发送速度太快，导致接收端的结束缓冲区很快的填充满了。此时如果发送端仍旧发送数据，那么接下来发送的数据都会丢包，继而导致丢包的一系列连锁反应，超时重传呀什么的。而TCP根据接收端对数据的处理能力，决定发送端的发送速度，这个机制就是流量控制。

在TCP协议的报头信息当中，有一个16位字段的窗口大小。在介绍这个窗口大小时我们知道，窗口大小的内容实际上是接收端接收数据缓冲区的剩余大小。这个数字越大，证明接收端接收缓冲区的剩余空间越大，网络的吞吐量越大。接收端会在确认应答发送ACK报文时，将自己的即时窗口大小填入，并跟随ACK报文一起发送过去。而发送方根据ACK报文里的窗口大小的值的改变进而改变自己的发送速度。如果接收到窗口大小的值为0，那么发送方将停止发送数据。并定期的向接收端发送窗口探测数据段，让接收端把窗口大小告诉发送端。  
![](https://img-blog.csdn.net/20180524111634561?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWNoZW54aWE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
注：16位的窗口大小最大能表示65535个字节（64K），但是TCP的窗口大小最大并不是64K。在TCP首部中40个字节的选项中还包含了一个窗口扩大因子M，实际的窗口大小就是16为窗口字段的值左移M位。每移一位，扩大两倍。  

### 拥塞控制  
TCP传输的过程中，发送端开始发送数据的时候，如果刚开始就发送大量的数据，那么就可能造成一些问题。网络可能在开始的时候就很拥堵，如果给网络中在扔出大量数据，那么这个拥堵就会加剧。拥堵的加剧就会产生大量的丢包，就对大量的超时重传，严重影响传输。

所以TCP引入了慢启动的机制，在开始发送数据时，先发送少量的数据探路。探清当前的网络状态如何，再决定多大的速度进行传输。这时候就引入一个叫做拥塞窗口的概念。发送刚开始定义拥塞窗口为 1，每次收到ACK应答，拥塞窗口加 1。在发送数据之前，首先将拥塞窗口与接收端反馈的窗口大小比对，取较小的值作为实际发送的窗口。

拥塞窗口的增长是指数级别的。慢启动的机制只是说明在开始的时候发送的少，发送的慢，但是增长的速度是非常快的。为了控制拥塞窗口的增长，不能使拥塞窗口单纯的加倍，设置一个拥塞窗口的阈值，当拥塞窗口大小超过阈值时，不能再按照指数来增长，而是线性的增长。在慢启动开始的时候，慢启动的阈值等于窗口的最大值，一旦造成网络拥塞，发生超时重传时，慢启动的阈值会为原来的一半（这里的原来指的是发生网络拥塞时拥塞窗口的大小），同时拥塞窗口重置为 1。  

![](https://img-blog.csdn.net/20180524125815394?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWNoZW54aWE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
拥塞控制是TCP在传输时尽可能快的将数据传输，并且避免拥塞造成的一系列问题。是可靠性的保证，同时也是维护了传输的高效性。



### **Q15**：数据库中已存在有序表，新的数据用什么数据结构算法插入，什么方式处理?
没找到

### **Q16**：数据库内联外联问题

![](https://img-blog.csdnimg.cn/20190117165920965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2Mzk1,size_16,color_FFFFFF,t_70)  
INNER JOIN（内联）：两个表a,b 相连接，取出符合连接条件的数据，数据集C

LEFT JOIN（左联）：先返回左表的所有行，再加上符合连接条件的匹配行，数据集A1+数据集C

RIGHT JOIN（右联）：先返回右表的所有行，再加上符合连接条件的匹配行，数据集B1+数据集C  

**内联**  

    select * from 放在左边的表 inner join 放在右边的表 on 左边表.字段a=右边表 .字段  
    select 左边表.显示字段,右边表.显示字段 from 放在左边的表 inner join 放在右边的表 on 左边表.字段a=右边表.  字段

**外联**  

    左联
    select * from 放在左边的表 left join 放在右边的表 on 左边表.字段a=右边表.字段
    左边为主表，右边为从表。根据on条件，如果匹配则显示字段内容，否则右边表内容显示null。
    右联
    select * from 放在左边的表 right join 放在右边的表 on 左边表.字段a=右边表.字段
    右边为主表，左边为从表。根据on条件，如果匹配则显示字段内容，否则左边表内容显示null。


### **Q17**：涉及两个终端之间使用什么协议，最能保证安全性
**TCP/IP协议**  
 TCP/IP（传输控制协议/网间协议）是一种网络通信协议，它规范了网络上的所有通信设备，尤其是一个主机与另一个主机之间的数据往来格式以及传送方式。TCP/IP是INTERNET的基础协议，也是一种电脑数据打包和寻址的标准方法。在数据传送中，可以形象地理解为有两个信封，TCP和IP就像是信封，要传递的信息被划分成若干段，每一段塞入一个TCP信封，并在该信封面上记录有分段号的信息，再将TCP信封塞入IP大信封，发送上网。在接受端，一个TCP软件包收集信封，抽出数据，按发送前的顺序还原，并加以校验，若发现差错，TCP将会要求重发。因此，TCP/IP在INTERNET中几乎可以无差错地传送数据。
### **Q18**：堆、栈原理
Java把内存划分成两种：一种是栈内存，一种是堆内存。   

在函数中定义的一些基本类型的变量和对象的引用变量都在函数的栈内存中分配。 

**当在一段代码块中定义一个变量时，java就在栈中为这个变量分配内存空间，**当超过变量的作用域后，java会自动释放掉为该变量分配的内存空间，该内存空间可以立刻被另作他用。
 
**堆内存用于存放由new创建的对象和数组。**  
在堆中分配的内存，由java虚拟机**自动垃圾回收器**来管理。在堆中产生了一个数组或者对象后，还可以在栈中定义一个特殊的变量，这个变量的取值等于数组或者对象在堆内存中的首地址，在栈中的这个特殊的变量就变成了数组或者对象的引用变量，以后就可以在程序中使用栈内存中的引用变量来访问堆中的数组或者对象，引用变量相当于为数组或者对象起的一个别名，或者代号。
 
引用变量是普通变量，定义时在栈中分配内存，引用变量在程序运行到作用域外释放。而数组＆对象本身在堆中分配，即使程序运行到使用new产生数组和对象的语句所在地代码块之外，数组和对象本身占用的堆内存也不会被释放，数组和对象在没有引用变量指向它的时候，才变成垃圾，不能再被使用，但是仍然占着内存，在随后的一个不确定的时间被垃圾回收器释放掉。这个也是java比较占内存的主要原因，实际上，栈中的变量指向堆内存中的变量，这就是 Java 中的指针!

## **Q19**：线程池，如何实现    
### 1.线程池的重要变量#

首先要定义一个存放所有线程的集合；  
另外，每有一个任务分配给线程池，我们就从线程池中分配一个线程处理它。但当线程池中的线程都在运行状态，没有空闲线程时，我们还需要一个队列来存储提交给线程池的任务。

    /**存放线程的集合*/
    private ArrayList<MyThead> threads;
    /**任务队列*/
    private ArrayBlockingQueue<Runnable> taskQueue; 

初始化一个线程池时，要指定这个线程池的大小；另外，我们还需要一个变量来保存已经运行的线程数目。  

    /**线程池初始限定大小*/
    private int threadNum;
    /**已经工作的线程数目*/
    private int workThreadNum;  

###  2.线程池的核心方法#
**线程池执行任务**  

接下来就是线程池的核心方法，每当向线程池提交一个任务时。如果已经运行的线程<线程池大小，则创建一个线程运行任务，并把这个线程放入线程池；否则将任务放入缓冲队列中。

    public void execute(Runnable runnable) {
        try {
            mainLock.lock();
            //线程池未满，每加入一个任务则开启一个线程
            if(workThreadNum < threadNum) {
                MyThead myThead = new MyThead(runnable);
                myThead.start();
                threads.add(myThead);
                workThreadNum++;
            }
            //线程池已满，放入任务队列，等待有空闲线程时执行
            else {
                //队列已满，无法添加时，拒绝任务
                if(!taskQueue.offer(runnable)) {
                    rejectTask();
                }
            }
        } finally {
            mainLock.unlock();
        }
    }

到这里，一个线程池已经实现的差不多了，我们还有最后一个难点要解决：从任务队列中取出任务，分配给线程池中“空闲”的线程完成。  
**分配任务给线程的第一种思路**  

很容易想到一种解决思路：额外开启一个线程，时刻监控线程池的线程空余情况，一旦有线程空余，则马上从任务队列取出任务，交付给空余线程完成。

这种思路理解起来很容易，但仔细思考，实现起来很麻烦  
1. 如何检测到线程池中的空闲线程   
2. 如何将任务交付给一个`.start()`运行状态中的空闲线程）。而且使线程池的架构变的更复杂和不优雅。 

**分配任务给线程的第二种思路**  

现在我们来讲第二种解决思路：线程池中的所有线程一直都是运行状态的，线程的空闲只是代表此刻它没有在执行任务而已；我们可以让运行中的线程，一旦没有执行任务时，就自己从队列中取任务来执行。

为了达到这种效果，我们要重写`run()`方法，所以要写一个自定义`Thread`类，然后让线程池都放这个自定义线程类  

    class MyThead extends Thread{
        private Runnable task;
        
        public MyThead(Runnable runnable) {
            this.task = runnable;
        }
        @Override
        public void run() {
            //该线程一直启动着，不断从任务队列取出任务执行
            while (true) {
                //如果初始化任务不为空，则执行初始化任务
                if(task != null) {
                    task.run();
                    task = null;
                }
                //否则去任务队列取任务并执行
                else {
                    Runnable queueTask = taskQueue.poll();
                    if(queueTask != null)
                        queueTask.run();    
                }
            }
        }
    } 

**3.最后生成的简单线程池** 

    /**
     * 自定义简单线程池
     */
    public class MyThreadPool{
    /**存放线程的集合*/
    private ArrayList<MyThead> threads;
    /**任务队列*/
    private ArrayBlockingQueue<Runnable> taskQueue;
    /**线程池初始限定大小*/
    private int threadNum;
    /**已经工作的线程数目*/
    private int workThreadNum;
    
    private final ReentrantLock mainLock = new ReentrantLock();
    
    public MyThreadPool(int initPoolNum) {
        threadNum = initPoolNum;
        threads = new ArrayList<>(initPoolNum);
        //任务队列初始化为线程池线程数的四倍
        taskQueue = new ArrayBlockingQueue<>(initPoolNum*4);
        
        threadNum = initPoolNum;
        workThreadNum = 0;
    }
    
    public void execute(Runnable runnable) {
        try {
            mainLock.lock();
            //线程池未满，每加入一个任务则开启一个线程
            if(workThreadNum < threadNum) {
                MyThead myThead = new MyThead(runnable);
                myThead.start();
                threads.add(myThead);
                workThreadNum++;
            }
            //线程池已满，放入任务队列，等待有空闲线程时执行
            else {
                //队列已满，无法添加时，拒绝任务
                if(!taskQueue.offer(runnable)) {
                    rejectTask();
                }
            }
        } finally {
            mainLock.unlock();
        }
    }
    
    private void rejectTask() {
        System.out.println("任务队列已满，无法继续添加，请扩大您的初始化线程池！");
    }
    public static void main(String[] args) {
        MyThreadPool myThreadPool = new MyThreadPool(5);
        Runnable task = new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行中");
            }
        };
        
        for (int i = 0; i < 20; i++) {
            myThreadPool.execute(task);
        }
    }
    
    class MyThead extends Thread{
        private Runnable task;
        
        public MyThead(Runnable runnable) {
            this.task = runnable;
        }
        @Override
        public void run() {
            //该线程一直启动着，不断从任务队列取出任务执行
            while (true) {
                //如果初始化任务不为空，则执行初始化任务
                if(task != null) {
                    task.run();
                    task = null;
                }
                //否则去任务队列取任务并执行
                else {
                    Runnable queueTask = taskQueue.poll();
                    if(queueTask != null)
                        queueTask.run();    
                }
            }
        }
    }
    }

现在我们来**总结**一下，这个自定义线程池的整个工作过程：

1. 初始化线程池，指定线程池的大小。  
2. 向线程池中放入任务执行。  
3. 如果线程池中创建的线程数目未到指定大小，则创建我们自定义的线程类放入线程池集合，并执行任务。执行完了后该线程会一直监听队列  
4. 如果线程池中创建的线程数目已满，则将任务放入缓冲任务队列  
5. 线程池中所有创建的线程，都会一直从缓存任务队列中取任务，取到任务马上执行  


### **Q20**：反射机制



**1.JAVA反射机制是在运行状态中**

对于任意一个类，都能够知道这个类的所有属性和方法；

对于任意一个对象，都能够调用它的任意一个方法和属性；

这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

**2.反射提供的功能：**

在运行时判断任意一个对象所属的类
在运行时构造任意一个类的对象
在运行时判断任意一个类所具有的成员变量和方法
在运行时调用任意一个对象的方法  

**（要想解剖一个类,必须先要获取到该类的字节码文件对象（class）。而解剖使用的就是Class类中的方法.所以先要获取到每一个字节码文件对应的Class类型的对象.）**

**3.关于class对象和这个class类**  
 
 Class对象的由来是将class文件读入内存，并为之创建一个Class对象   

![](https://img-blog.csdnimg.cn/201811202314170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcyNDQ2Nw==,size_16,color_FFFFFF,t_70)
**4.class类:**代表一个类，是Java反射机制的起源和入口  

- 用于获取与类相关的各种信息， 提供了获取类信息的相关方法
- Class类继承自Object类
- Class类是所有类的共同的图纸
- 每个类有自己的对象，同时每个类也看做是一个对象，有共同的图纸Class,存放类的结构信息，能够通过相应方法取出相应的信息：类的名字、属性、方法、构造方法、父类和接口。

![](https://img-blog.csdnimg.cn/2018112023202147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcyNDQ2Nw==,size_16,color_FFFFFF,t_70)  

Class 类的实例表示正在运行的 Java 应用程序中的类和接口。也就是jvm中有N多的实例每个类都有该Class对象。（包括基本数据类型）  

Class 没有公共构造方法。Class 对象是在加载类时由 Java 虚拟机以及通过调用类加载器中的defineClass 方法自动构造的。也就是这不需要我们自己去处理创建，JVM已经帮我们创建好了。

没有公共的构造方法，方法共有64个。  

**5.反射的使用场景**

- Java编码时知道类和对象的具体信息，此时直接对类和对象进行操作即可，无需反射
- 如果编码时不知道类或者对象的具体信息，此时应该使用反射来实现  

  比如类的名称放在XML文件中，属性和属性值放在XML文件中，需要在运行时读取XML文件，动态获取类的信息
在编译时根本无法知道该对象或类可能属于哪些类，程序只依靠运行时信息来发现该对象和类的真实信息 

### **Q21**： 编程题，从一组数中，有正有负，选取三个数等于0的情况，把所有情况打印出来

### Q22：http与tcp的区别与联系
## 一、TCP连接概念 #

手机能够使用联网功能是因为手机底层实现了TCP/IP协议，可以使手机终端通过无线网络建立TCP连接。TCP协议可以对上层网络提供接口，使上层网络数据的传输建立在“无差别”的网络之上。

建立起一个TCP连接需要经过“三次握手”：

      第一次握手：客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；

      第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

      第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

      握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，
      TCP 连接都将被一直保持下去。断开连接时服务器和客户端均可以主动发起断开TCP连接的请求，断开过程需要经过“四次握手”（过程就不细写 了，就是服务器和客户端
      交互，最终确定断开）

### 二、HTTP连接概念
HTTP协议即超文本传送协议(Hypertext Transfer Protocol )，是Web联网的基础，也是手机联网常用的协议之一，HTTP协议是建立在TCP协议之上的一种应用。

HTTP连接最显著的特点是客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。从建立连接到关闭连接的过程称为“一次连接”。

    1）在HTTP 1.0中，客户端的每次请求都要求建立一次单独的连接，在处理完本次请求后，就自动释放连接。

    2）在HTTP 1.1中则可以在一次连接中处理多个请求，并且多个请求可以重叠进行，不需要等待一个请求结束后再发送下一个请求。

  由于HTTP在每次请求结束后都会主动释放连接，因此HTTP连接是一种“短连接”，要保持客户端程序的在线状态，需要不断地向服务器发起连接请求。通常的 做法是即时不需要获得任何数据，客户端也保持每隔一段固定的时间向服务器发送一次“保持连接”的请求，服务器在收到该请求后对客户端进行回复，表明知道客 户端“在线”。若服务器长时间无法收到客户端的请求，则认为客户端“下线”，若客户端长时间无法收到服务器的回复，则认为网络已经断开。

### 三、区别与联系  
TCP对应于传输层，HTTP对应于应用层，从本质上来说，二者没有可比性。  

    1.TCP是底层协议，定义的是数据传输和连接方式的规范。 
    2.HTTP是应用层协议，定义的是传输数据的内容的规范。
    3.HTTP协议中的数据是利用TCP协议传输的，所以支持HTTP就一定支持TCP。
Http协议是建立在TCP协议基础之上的，当浏览器需要从服务器获取网页数据的时候，会发出一次Http请求。Http会通过TCP建立起一个到服务器的连接通道，当本次请求需要的数据完毕后，Http会立即将TCP连接断开，这个过程是很短的。所以Http连接是一种短连接，是一种无状态的连接。

### Q25：TCP与UDP区别

### 一、TCP/IP网络模型
计算机与网络设备要相互通信，双方就必须基于相同的方法。比如，如何探测到通信目标、由哪一边先发起通信、使用哪种语言进行通信、怎样结束通信等规则都需要事先确定。不同的硬件、操作系统之间的通信，所有的这一切都需要一种规则。而我们就把这种规则称为协议（protocol）。

TCP/IP 是互联网相关的各类协议族的总称，比如：TCP，UDP，IP，FTP，HTTP，ICMP，SMTP 等都属于 TCP/IP 族内的协议。

TCP/IP模型是互联网的基础，它是一系列网络协议的总称。这些协议可以划分为四层，分别为链路层、网络层、传输层和应用层。

    链路层：负责封装和解封装IP报文，发送和接受ARP/RARP报文等。  
    网络层：负责路由以及把分组报文发送给目标网络或主机。  
    传输层：负责对报文进行分组和重组，并以TCP或UDP协议格式封装报文。  
    应用层：负责向用户提供应用程序，比如HTTP、FTP、Telnet、DNS、SMTP等。  


![](https://image.fundebug.com/2019-03-21-01.png) 

在网络体系结构中网络通信的建立必须是在通信双方的对等层进行，不能交错。 在整个数据传输过程中，数据在发送端时经过各层时都要附加上相应层的协议头和协议尾（仅数据链路层需要封装协议尾）部分，也就是要对数据进行协议封装，以标识对应层所用的通信协议。接下去介绍TCP/IP 中有两个具有代表性的传输层协议----TCP 和 UDP。

### 二、UDP

UDP协议全称是用户数据报协议，在网络中它与TCP协议一样用于处理数据包，是一种无连接的协议。在OSI模型中，在第四层——传输层，处于IP协议的上一层。UDP有不提供数据包分组、组装和不能对数据包进行排序的缺点，也就是说，当报文发送之后，是无法得知其是否安全完整到达的。

它有以下几个特点：  

## 1. 面向无连接 ##
首先 UDP 是不需要和 TCP一样在发送数据前进行三次握手建立连接的，想发数据就可以开始发送了。并且也只是数据报文的搬运工，不会对数据报文进行任何拆分和拼接操作。

具体来说就是：  

    在发送端，应用层将数据传递给传输层的 UDP 协议，UDP 只会给数据增加一个 UDP 头标识下是 UDP 协议，然后就传递给网络层了  
    在接收端，网络层将数据传递给传输层，UDP 只去除 IP 报文头就传递给应用层，不会任何拼接操作  

### 2. 有单播，多播，广播的功能

UDP 不止支持一对一的传输方式，同样支持一对多，多对多，多对一的方式，也就是说 UDP 提供了单播，多播，广播的功能。  

## 3. UDP是面向报文的  

发送方的UDP对应用程序交下来的报文，在添加首部后就向下交付IP层。UDP对应用层交下来的报文，既不合并，也不拆分，而是保留这些报文的边界。因此，应用程序必须选择合适大小的报文

## 4. 不可靠性

首先不可靠性体现在无连接上，通信都不需要建立连接，想发就发，这样的情况肯定不可靠。

并且收到什么数据就传递什么数据，并且也不会备份数据，发送数据也不会关心对方是否已经正确接收到数据了。

再者网络环境时好时坏，但是 UDP 因为没有拥塞控制，一直会以恒定的速度发送数据。即使网络条件不好，也不会对发送速率进行调整。这样实现的弊端就是在网络条件不好的情况下可能会导致丢包，但是优点也很明显，在某些实时性要求高的场景（比如电话会议）就需要使用 UDP 而不是 TCP。
  
![](https://image.fundebug.com/2019-03-21-02.gif)  

从上面的动态图可以得知，UDP只会把想发的数据报文一股脑的丢给对方，并不在意数据有无安全完整到达。  

## 5. 头部开销小，传输数据报文时是很高效的。 ##
![](https://image.fundebug.com/2019-03-21-03.png)  

UDP 头部包含了以下几个数据：

    两个十六位的端口号，分别为源端口（可选字段）和目标端口  
    整个数据报文的长度   
    整个数据报文的检验和（IPv4 可选 字段），该字段用于发现头部信息和数据中的错误  
因此 UDP 的头部开销小，只有八字节，相比 TCP 的至少二十字节要少得多，在传输数据报文时是很高效的  

# 三、TCP  #

当一台计算机想要与另一台计算机通讯时，两台计算机之间的通信需要畅通且可靠，这样才能保证正确收发数据。例如，当你想查看网页或查看电子邮件时，希望完整且按顺序查看网页，而不丢失任何内容。当你下载文件时，希望获得的是完整的文件，而不仅仅是文件的一部分，因为如果数据丢失或乱序，都不是你希望得到的结果，于是就用到了TCP。

TCP协议全称是传输控制协议是一种面向连接的、可靠的、基于字节流的传输层通信协议，由 IETF 的RFC 793定义。TCP 是面向连接的、可靠的流协议。流就是指不间断的数据结构，你可以把它想象成排水管中的水流。

**1. TCP连接过程**   
如下图所示，可以看到建立一个TCP连接的过程为（三次握手的过程）:  
![](https://image.fundebug.com/2019-03-21-04.png)  
**第一次握手**

客户端向服务端发送连接请求报文段。该报文段中包含自身的数据通讯初始序号。请求发送后，客户端便进入 SYN-SENT 状态。

**第二次握手**

服务端收到连接请求报文段后，如果同意连接，则会发送一个应答，该应答中也会包含自身的数据通讯初始序号，发送完成后便进入 SYN-RECEIVED 状态。

**第三次握手**

当客户端收到连接同意的应答后，还要向服务端发送一个确认报文。客户端发完这个报文段后便进入 ESTABLISHED 状态，服务端收到这个应答后也进入 ESTABLISHED 状态，此时连接建立成功。

这里可能大家会有个疑惑：为什么 TCP 建立连接需要三次握手，而不是两次？这是因为这是为了防止出现失效的连接请求报文段被服务端接收的情况，从而产生错误。  

![](https://image.fundebug.com/2019-03-21-05.gif)  

**2. TCP断开链接**
  
![](https://image.fundebug.com/2019-03-21-06.png)  
TCP 是全双工的，在断开连接时两端都需要发送 FIN 和 ACK。

**第一次握手**

若客户端 A 认为数据发送完成，则它需要向服务端 B 发送连接释放请求。

**第二次握手**

B 收到连接释放请求后，会告诉应用层要释放 TCP 链接。然后会发送 ACK 包，并进入 CLOSE_WAIT 状态，此时表明 A 到 B 的连接已经释放，不再接收 A 发的数据了。但是因为 TCP 连接是双向的，所以 B 仍旧可以发送数据给 A。

**第三次握手**

B 如果此时还有没发完的数据会继续发送，完毕后会向 A 发送连接释放请求，然后 B 便进入 LAST-ACK 状态。

**第四次握手**

A 收到释放请求后，向 B 发送确认应答，此时 A 进入 TIME-WAIT 状态。该状态会持续 2MSL（最大段生存期，指报文段在网络中生存的时间，超时会被抛弃） 时间，若该时间段内没有 B 的重发请求的话，就进入 CLOSED 状态。当 B 收到确认应答后，也便进入 CLOSED 状态。  

**3. TCP协议的特点**  

- 面向连接  
 
        面向连接，是指发送数据之前必须在两端建立连接。建立连接的方法是“三次握手”，这样能建立可靠的连接。建立连接，是为数据的可靠传输打下了基础。

- 仅支持单播传输  

        每条TCP传输连接只能有两个端点，只能进行点对点的数据传输，不支持多播和广播传输方式。  
- 面向字节流

        TCP不像UDP一样那样一个个报文独立地传输，而是在不保留报文边界的情况下以字节流方式进行传输。

- 可靠传输

        对于可靠传输，判断丢包，误码靠的是TCP的段编号以及确认号。TCP为了保证报文传输的可靠，就给每个包一个序号，同时序号也保证了传送到接收端实体的包的按序接收。
        然后接收端实体对已成功收到的字节发回一个相应的确认(ACK)；如果发送端实体在合理的往返时延(RTT)内未收到确认，那么对应的数据（假设丢失了）将会被重传。  

- 提供拥塞控制

        当网络出现拥塞的时候，TCP能够减小向网络注入数据的速率和数量，缓解拥塞

- TCP提供全双工通信

        TCP允许通信双方的应用程序在任何时候都能发送数据，因为TCP连接的两端都设有缓存，用来临时存放双向通信的数据。当然，TCP可以立即发送一个数据段，也可以缓存
        一段时间以便一次发送更多的数据段（最大的数据段大小取决于MSS）

# 四、TCP和UDP的比较  

**1、对比**  
![](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=261857941,1558384996&fm=26&gp=0.jpg)  

**2.总结**  
1、TCP面向连接（如打电话du要先拨号建立连接；UDP是无连接的，即发送数据zhi之前不需dao要建立连接

2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付

Tcp通过校验和，重传控制，序号标识，滑动窗口、确认应答实现可靠传输。如丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。

3、UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。

4.每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信

5、TCP对系统资源要求较多，UDP对系统资源要求较少。