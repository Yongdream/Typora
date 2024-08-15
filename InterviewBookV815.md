# Linux

## Linux中，vim里替换一段字符串怎么做

在 Vim 中替换一段字符串可以使用 `:s` 命令。以下是具体步骤：

1. 打开 Vim 并进入你需要编辑的文件。

2. 确保你处于正常模式（按 `Esc` 键可以进入正常模式）。

3. 使用以下格式的命令进行替换：
   - **替换当前行的字符串：**
     
     ```vim
     :s/旧字符串/新字符串/g
     ```
     
   - **替换整个文件的字符串：**
     
     ```vim
     :%s/旧字符串/新字符串/g
     ```
   - **替换第 n 行开始到最后一行中每一行的第一个 str1 为 str2**
     
     ```vim
     :n,$s/str1/str2/
     ```
   
   - **替换第 n 行开始到最后一行中所有的 str1 为 str2**
   
     ```vim
     :n,$s/str1/str2/g
     ```
   

这些命令中的 `/g` 表示全局替换，也就是替换行中的所有匹配项。去掉 `/g` 只会替换每行中第一个匹配项。

## 在Linux下查看进程所耗资源

- **`top`:**用于实时监视系统中运行的进程。可以使用top命令查看各个进程所占用CPU、内存、虚拟内存、磁盘I/O等资源占用情况；
- **`PS`:**⽤于列出当前系统中运行的进程。可以使用ps命令查看各进程的 PID、CPU 占用、内存占用、状态等信息  
- **`htop`:**是top命令的升级版，提供了更丰富的功能和界面。可以使用htop命令查看进程的CPU 占用、内存占用、线程数量、进程状态等信息 
- **`pidstat`:**用于显示进程的资源使用情况。可以使用pidstat命令查看各进程的 CPU 使用情况、内存使用情况、I/O 操作情况等  
- **`iostat`:**用于监视系统的磁盘 I/O 性能。可以使用iostat命令查看各进程的磁盘 I/O 操作情况，包括每个进程的读写速度、请求队列长度、磁盘利用率等。  

## Linux中查找某个字符串

在 Linux 中，可以使用 `grep` 命令来查找某个字符串。以下是一些常见的用法和示例：

假设你有一个文件 `example.txt`，要在其中查找字符串 `hello`：

```sh
grep "hello" example.txt
```

要在当前目录及其子目录中的所有文件中查找字符串 `hello`，可以使用 `-r` 选项：

```sh
grep -r "hello" .
```

## 删除一行

```vim
" 直接删除
dd

" 使用:<行号>d命令：
:3d
```

## `netstat`能查看什么协议

- TCP
- UDP
- Unix域套接字：本地进程中Socket通信
- RAW协议：RAW协议是一种基本的传输层协议，提供对IP层的直接访问
- ICMP：ICMP是一种网络层协议，用于发送控制和错误信息

## 波浪号 `~` 

波浪号 `~` 用作路径中的一个特殊符号，代表当前用户（即执行命令的用户）的家目录（home directory）。

**示例**：

- 如果当前用户是 `ljm`，那么 `~/.ssh` 就表示 `/home/ljm/.ssh`。

- 如果你以用户 `root` 执行命令，`~` 就代表 `/root`。

  ```bash
  cd ~           # 切换到当前用户的家目录
  ls ~/Documents # 列出当前用户家目录下的 Documents 文件夹内容
  cp file.txt ~   # 将 file.txt 复制到当前用户的家目录
  ```

## linux命令|chmod，scp，chattr，pgrep，awk

1. `chmod` 更改文件或目录的权限。

   ```bash
   # 示例：为文件 example.txt 添加可执行权限
   chmod +x example.txt
   
   # 示例：设置文件 example.txt 的权限为 644（所有者可读写，其他用户只读）
   chmod 644 example.txt
   ```

2. `scp` 安全地复制文件或目录到远程主机或从远程主机复制到本地

   ```bash
   scp [选项] [源文件或目录] [目标]
   # 将本地文件 file.txt 复制到远程主机的 /home/user/ 目录
   scp file.txt user@remotehost:/home/user/
   # 从远程主机复制文件 file.txt 到本地当前目录
   scp user@remotehost:/home/user/file.txt .
   ```

3. `chattr` 更改文件或目录的属性

   ```bash
   chattr [选项] [属性] [文件或目录]
   # 设置文件 important.txt 为不可更改
   chattr +i resolv.conf
   # 取消 important.txt 的不可更改属性 命令防⽌系统中某个关键⽂件被修改
   chattr -i resolv.conf
   ```

4. `pgrep` 根据进程名称或其他属性查找进程的进程 ID

   **`-l`** 列出与模式匹配的进程 ID 及其名称

   **`-a`** 列出所有与模式 nginx 匹配的进程 ID 和完整路径

   ```bash
   pgrep [选项] [模式]
   
   pgrep -l nginx
   1234 nginx
   5678 nginx
   
   pgrep -a nginx
   1234 /usr/sbin/nginx -g daemon on; master_process on;
   5678 /usr/sbin/nginx -g daemon on; master_process on;
   ```

5. `awk` 一个强大的文本处理工具，通常用于模式匹配和数据提取

   ```bash
   awk '脚本' [文件]
   
   # 示例：打印文件 data.txt 的第一列
   awk '{print $1}' data.txt
   
   # 示例：打印文件 data.txt 中第2列大于100的行
   awk '$2 > 100' data.txt
   
   # 示例：统计文件 data.txt 中每个唯一单词的出现次数
   awk '{for(i=1;i<=NF;i++) word[$i]++} END {for(w in word) print w, word[w]}' data.txt
   ```

## 程序在后台运行

1. **加 `&` 符号**

   在命令的末尾加上 `&`，可以将该命令放到后台执行

   ```bash
   # 执行文件
   ./test.py &
   # 执行该命令后，Linux 会立即返回后台任务的 PID
   
   # 查看是否在后台运行
   ps -ef|grep test
    
   # 后台的程序 需要关闭时，需要kill命令停止
   killall [程序名] 
   ```

2. **使用 `nohup`**

   如果要在终端**会话终止后保持任务继续处于活动状态**，`nohup` 就是首选工具。`nohup` 是「no hang up」的缩写，它可以确保命令的不间断执行。

   ```bash
   nohup ./example.sh &
   ```

   `nohup` 会忽略所有 SIGHUP 信号，程序的标准输出和标准错误通常会被重定向到 `nohup.out` 文件中

   ```bash
   nohup example.sh > myoutput.log 2>&1 &
   ```

3. **使用 screen 在后台运行 Linux 命令和管理会话**

   `screen` 是一个强大的 Linux 实用程序，专为高级会话管理而设计。它能够创建多个终端会话，并轻松地在会话之间切换，还可以从当前会话中分离出来以便稍后重新连接

   ```bash
   # 创建一个新的 screen 会话
   screen -S session_name
   # 从当前会话中分离
   Ctrl + a，然后按 d
   # 重新连接到之前分离的会话
   screen -r session_name
   ```

4. **使用 `Ctrl+Z` 暂停进程，并用 `bg` 将其放到后台**

   - 按 **`Ctrl+Z`** 暂停进程

   ```bash
   $ pct
   
   # 键盘输入Ctrl + Z
   ^Z
   [1]+ Stopped    pct
   ```

   - **`jobs`**

     **基本用法**：`jobs` 命令显示当前 shell 会话中所有后台任务的列表及其状态。
   
     **状态标识**：
   
     - `+` （加号）：表示当前的作业。
     - `-` （减号）：表示当前作业之后的一个作业。
   
     **状态**：
   
     - `Running`：正在运行的任务。
     - `Stopped`：被暂停的任务。
     - `Terminated`：已终止的任务（通常由 `kill` 命令终止）。
   
   ```bash
   $ jobs -l
   
   [1]+ 12345 Stopped           pct
   [2]- 12346 Running           ping google.com &
   # `[1]+` 表示当前的作业（`pct`）
   # `[2]-` 表示当前作业之后的一个作业（`ping google.com`）
   ```
   
   - **`bg`** 在后台暂停的命令，变成继续执行
   
   ```bash
   $ bg %1      # 将作业号为 1 的进程放到后台运行
   $ jobs -l
   [1]+ 12345 Running           pct &
   [2]- 12346 Running           ping google.com &
   ```
   
   - **`fg`** 将后台中的命令调至前台继续运行
   
   ```bash
   $ fg %1
   $ jobs -l
   [1]+ 12345 Running           pct
   [2]- 12346 Running           ping google.com &
   ```

## 查找一个字符串是否在filename文件中

```bash
grep string filename
awk '/string/ {print NR":", $0}' filename

$ less filename
$ vi filename
# 在 less 环境中，输入 /string 后按“Enter”，less 会高亮显示第一个匹配项，之后可以按 n 跳转到下一个匹配项，按 N 跳转到前一个匹配项
```



## Git|merge时候遇到conflict怎么办

进行协作开发时，不同分支对同一文件的同一区域进行了修改

Git 会提示你哪些文件存在冲突，并在这些文件中插入冲突标记。需要手动解决冲突，选择保留哪部分代码，或合并两部分代码，然后filename.txt中**删除冲突标记**。重新add commit

```sh
<<<<<<< HEAD
// 你的当前分支中的代码
=======
<<<<<<<
冲突的分支中的代码
>>>>>>> branch-name
```

---



# C++

## c++基础|关键字|静态static 和 常量const 的区别  

### 1. 修饰局部变量

#### `const` 修饰局部变量

- **作用**：表示该变量为只读，其值不能被修改。

  ```cpp
  void function() {
      const int localVar = 10;
      // localVar = 20;  // 错误，不能修改
  }
  ```

##### `static` 修饰局部变量

- **作用**：改变变量的生命周期，使其在超出作用域后仍然存在，直到程序结束时才被销毁。

  ```cpp
  void function() {
      static int counter = 0;
      counter++;
      // counter 的值在函数调用之间保持
  }
  ```

### 2. 修饰全局变量

#### `const` 修饰全局变量

- **作用**：如果全局变量仅在一个文件中使用，那么 `const` 的作用与局部变量类似，表示该变量为只读。

  ```cpp
  const int globalVar = 100;
  ```

#### `static` 修饰全局变量

- **作用**：使得全局变量只能在本源文件中使用，其他源文件不能访问，从而实现内部链接。存储在静态存储区，运行期间一直存在

  ```cpp
  static int fileScopeVar = 0;  // 仅在当前文件中可见
  ```

### 3. 修饰类成员变量

#### `const` 修饰类成员变量

- **作用**：表示成员变量为只读，不能被修改，必须在**`构造函数初始化列表中初始化`**。

  ```cpp
  class MyClass {
  public:
      const int constMember;
      MyClass(int value) : constMember(value) {
          // constMember = value;  // 错误，不能在构造函数体内赋值
      }
  };
  ```

#### `static` 修饰类成员变量

- **作用**：使成员变量**属于类而不是某个对象，所有对象共享静态成员变量**，必须在类外定义。

  ```cpp
  class MyClass {
  public:
      static int staticMember;	// 类内声明
      // static int staticMember = 10;  // 错误：在类内定义和初始化静态成员变量
  };
  
  int MyClass::staticMember = 0;  // 类外定义
  ```

### 4. 修饰类成员函数

#### `const` 修饰类成员函数

- **作用**：表明该成员函数**`不能修改类的任何成员变量`**，实际是修饰该成员函数隐含的 `this` 指针。

  ```cpp
  class MyClass {
  public:
      void myFunction() const {
          // 不能修改成员变量
      }
  };
  ```

#### `static` 修饰类成员函数

- **作用**：使成员函数属于类而不是某个对象，没有 `this` 指针，**不能访问任何非静态成员**。

  ```cpp
  class MyClass {
  public:
      static void staticFunction() {
          // 不能访问非静态成员
      }
  };
  ```

### 5. 修饰类对象和类

#### `const` 修饰类对象

- **作用**：表示该对象中的所有成员变量均不可被修改，只能调用 `const` 成员函数。

  ```cpp
  const MyClass obj;
  obj.constMemberFunction();  // 只能调用 const 成员函数
  ```

#### `static` 修饰类和内部类

- **作用**：`static` 不能修饰普通类，但是**可以修饰内部类**，使其可以在不实例化外部类的情况下使用。

  ```cpp
  class OuterClass {
  public:
      static class InnerClass {
      public:
          void innerFunction() {
              // 内部类函数
          }
      };
  };
  
  OuterClass::InnerClass innerObj;
  innerObj.innerFunction();
  ```

### 总结

- **`const`**：用于表示不可修改的常量。可以修饰局部变量、全局变量、类成员变量、类成员函数以及类对象，保证数据的只读属性。
- **`static`**：用于控制变量或函数的生命周期和作用域。可以修饰局部变量、全局变量、类成员变量和类成员函数，提供共享和内部链接功能。**静态成员变量和函数的定义（实现）通常在类定义之外进行，以确保它们在程序中只存在一个实例。**

通过理解和正确使用 `const` 和 `static` 关键字，可以编写出更安全、性能更高、可维护性更强的代码。

## c++基础|关键字|final 修饰符的作用

- 修饰类，该类无法被继承

- 修饰类的成员函数，成员函数不能被派生重写

- 提高代码的稳定性和可维护性

## c++基础|关键字|`inline`

`inline`关键字在C++中引入是为了替代C语言中的宏函数，提供一种类型安全、可调试的优化方式。

- 编译器行为
  - 在编译阶段，编译器会尝试将`inline`函数的调用替换为函数体本身，从而减少函数调用的开销。
  - 这是对编译器的建议，编译器可以根据具体情况决定是否内联。
- 优点
  - **性能提升**：省去函数调用的开销，包括参数压栈、栈帧开辟与回收、返回结果等
  - **类型安全**：与宏定义相比，`inline`函数在代码展开时会进行类型检查和自动类型转换，避免了宏定义中常见的错误。
  - **类成员函数：**在类中声明同时定义的成员函数会自动转化为内联函数，因此可以直接访问类的成员变量。这是宏定义无法实现的。
  - **可调试：**`inline`函数在运行时可以进行调试，而宏定义在预处理阶段展开后无法进行调试。
- 缺点
  - **代码膨胀：**每次调用`inline`函数都需要复制函数体，增加了代码量和内存消耗。
  - **库函数升级问题：**`inline`函数不会随着库函数的升级而自动升级，用户需要==**重新编译代码**==。非`inline`函数的库升级只需重新链接，而不需要重新编译。
  - **内联不可控：**编译器对内联函数具有最终决定权

## c++基础|关键字|`void*`

void*定义一个指针变量，但不说明他指向哪一个类型。即这个指针变量只有地址，没有大小

**函数模板**：`void*`可以用作函数模板的通用参数，使得函数可以处理不同类型的数据。

**空指针**：`void*`可以用来表示一个空指针，类似于`NULL`。`(void*)0`通常表示一个空地址，不指向任何有效的内存位置。

**动态内存分配**：`void*`常用于动态内存分配函数（如`malloc`、`calloc`）的返回类型。这样可以分配任意类型的内存块，并将其指针存储在`void*`类型的指针中。之后，程序可以将`void*`转换为所需的类型进行操作。

```c++
void* ptr = malloc(100);  // 分配100字节的内存
int* intPtr = static_cast<int*>(ptr);  // 转换为int* 类型
```

## c++基础|关键字|`sizeof`

 **\*有无unsigned修饰都一样**

| 字节 | short | int  | float | long | *(地址) | double | long long |
| :--: | :---: | :--: | :---: | :--: | :-----: | :----: | :-------: |
| 32位 |   2   |  4   |   4   |  4   |    4    |   8    |     8     |
| 64位 |   2   |  4   |   4   |  8   |    8    |   8    |     8     |



## C++基础|编译过程

预处理 「Preprocessing」，编译「Compilation」，汇编「Assemble」，链接「Linking」

![img](D:\Yang7hi\note\typora_photo\InterviewBook\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzY1NTM3,size_16,color_FFFFFF,t_70.png)

## C++基础|静态链接 动态链接

LIB（**Static Library**）是编译时用到的，DLL（**Dynamic Link Library**）是运行时用到的。如果要完成源代码的编译，只需要LIB；如果要使动态链接的程序运行起来，只需要DLL。

**静态链接`静态库（.a，.lbb）`**就是把Lib文件中用到的函数直接链接进目标程序，程序运行时不在需要调用其他的库文件。

- 空间浪费：每个执行文件所需要的目标文件都需要一个样本；
- 重新编译：每当库函数代码修改，需要重新编译形成可执行程序；
- 运行效率：因已具备索要执行程序所需东西，运行速度快

**动态链接`动态库（.so，.dll）`**，导入库（LIB）文件包含了调用函数在动态链接库（DLL）中的位置信息，这些信息在编译时被用来链接目标程序。程序在运行时会根据这些信息从DLL文件中动态加载相应的函数代码。

- 代码共享，空间少：多个程序执行时共享一个副本；
- 程序升级方便：只要替换文件，无需重新编译；
- 性能损耗大：把链接推迟到运行时

## C++面向对象|Object Oriented Programming，简称为OOP

**面向过程**强调通过一系列步骤或程序（过程）来解决问题，程序是按照从上到下的顺序执行的。

**面向对象编程**以对象为中心的编程范式，强调使用对象和类来组织代码

**声明式编程**是一种描述“是什么”的编程范式，而不是“如何做”。SQL和HTML是声明式编程的典型例子。

事件驱动编程，常用于图形用户界面（GUI）应用和实时系统，事件（用户交互或其他外部输入触发的动作）驱动

## C++面向对象|的三大特性是**封装**、**继承**和**多态**

- **继承**
  继承允许一个类（派生类）继承另一个类（基类）的属性和方法，促进代码重用和扩展。子类可以使用父类的所有功能，并在此基础上进行扩展。继承的过程是从一般到特殊的过程。通过继承，可以实现代码的重用，并且建立类之间的层次关系。

  - **实现继承**：子类直接使用父类的属性和方法，无需额外编码。

  - **接口继承**：子类继承父类的属性和方法的名称，但提供自己的实现方式。基类作为纯虚类（接口），子类实现接口中的方法。

    ```cpp
    class Base {
    public:
        virtual void show() = 0; // 纯虚函数，没有具体实现
    };
    
    class Derived : public Base {
    public:
        void show() override {
            std::cout << "Derived class show method" << std::endl;
        }	// 接口继承
        void display() {
            std::cout << "Derived class display method" << std::endl;
        }	// 实现继承
    };
    
    ```

- **封装**
  将对象的状态（属性）和行为（方法）打包在一起，并隐藏内部实现细节，只暴露必要的接口给外部。提高代码的可维护性和可复用性。

- **多态**
  多态是指一个接口可以被多个类实现。向不同对象发送相同的消息，不同对象会**`根据自身的特性响应不同的行为`**。

- Overload（重载）：函数名相同，参数类型或顺序不同的函数构成重载。

- Override（重写）：派生类覆盖基类用virtual声明的成员函数。       
- Overwrite（隐藏）：派生类的函数屏蔽了与其同名的基类函数。派生类的函数与基类函数同名，但是参数不同，隐藏基类函数。如果参数相同，但是基类没有virtual关键字，基类函数将被隐藏。

  - 编译期间就可以确定函数的调用地址，并产生代码，则是静态的，即地址早绑定 。
  - 如果函数的调用地址需要在运行时才能确定，则是晚绑定 。
  - *确定函数调用是编译期间还是运行期间确定的，可以通过分析函数是否为虚函数、指针或引用的类型、以及编译器生成的代码等因素：1. 非虚函数（早绑定）虚函数（晚绑定）；2.指针和虚函数*

  **静态多态（早绑定 函数重载）**

  ```cpp
  class Print {
  public:
      void show(int i) { /* 打印整数 */ }
      void show(double f) { /* 打印浮点数 */ }
  };
  ```

  **动态多态（晚绑定 虚函数）**

  ```cpp
  class Animal {
  public:
      virtual void makeSound() { /* 通用声音 虚函数 */ }
  };
  
  class Dog : public Animal {
  public:
      void makeSound() override { /* 狗叫 */ }
  };
  
  class Cat : public Animal {
  public:
      void makeSound() override { /* 猫叫 */ }
  };
  ```

- **模块化（不属于三大特性，但是是面向对象的特点**

  面向对象的设计方法鼓励将程序划分为相互独立的模块或对象，每个模块负责特定功能。这种模块化的设计使得程序更容易理解、维护和调试。同时，模块化促进协作，不同成员独立开发

## C++面向对象|多态和继承在什么情况下使用

这些都是需要尽可能⼤的进⾏代码重⽤的时候⽤到的。  

继承和多态经常⼀起使⽤。通过继承，可以建⽴类之间的层次结构；通过多态，可以根据对象的实际类型来调⽤适当的⽅法。这样可以提⾼代码的可重⽤性、可扩展性和可维护性，使代码更加灵活和易于扩展。  

继承通常⽤于描述"is-a"的关系，⽐如⼀个⼦类可以继承⽗类的特征和⾏为，并且可以添加⾃⼰的特定特征和⾏为。继承的主要优点是代码的重⽤和扩展性。当存在明确的层次关系和共享代码的需求时，可以使⽤继承。

多态的主要优点是增加了代码的灵活性和可扩展性。当需要根据不同的对象类型来执⾏不同的操作时，可以使⽤多态。

## C++面向对象|除了多态和继承还有什么面向对象方法 （封装、接口、重载）

除了多态和继承，面向对象编程还包括封装、抽象、重载和组合等概念和技术。

1. 封装（Encapsulation）

   封装是将数据和操作数据的方法封装在一个类中，并通过访问控制（如public、protected、private）隐藏类的内部实现细节，只暴露必要接口。保护对象的内部状态。提高代码的可维护性。

2. 抽象（Abstraction）

   抽象是将复杂的现实世界问题简化为模型，通过提取公共特性，忽略不必要的细节

3. 接口（Interface）

   接口是一组方法的集合。接⼝提供了⼀种规范，规定了类应该实现哪些⽅法，并描述了这些⽅法应该做什么 

4. 重载（Overloading）

   重载是允许在同一个作用域中定义多个同名函数，但这些函数具有不同的参数列表（参数的类型、数量或顺序不同）。重载包括函数重载和运算符重载。
   

## C++面向对象|多态实现的注意点

### 1. 使用虚函数和`override`关键字

在基类中声明虚函数，以便派生类可以覆盖这些函数实现多态性，并使用`override`关键字明确表示派生类中的函数是重写基类中的虚函数。

```cpp
class Base {
public:
    virtual void display() const {
        std::cout << "Base display" << std::endl;
    }
    virtual ~Base() = default; // 确保基类有虚析构函数
};

class Derived : public Base {
public:
    void display() const override {
        std::cout << "Derived display" << std::endl;
    }
};
```

### 2. 确保基类有虚析构函数

如果基类没有虚析构函数，使用基类指针删除派生类对象时会导致未定义行为。

```cpp
class Base {
public:
    virtual ~Base() { // 确保基类析构函数是虚函数
        std::cout << "Base destructor" << std::endl;
    }
};
```

### 3. 避免对象切片

当**将派生类对象赋值给基类对象**时，会发生对象切片。应**使用基类指针或引用来避免**这个问题。

**<a href="##C++面向对象|对象切片示例">🔗对象切片</a>**

### 4. 理解纯虚函数和抽象类

纯虚函数用于定义抽象类，不能实例化抽象类，只能派生子类并实现所有纯虚函数。

```cpp
class AbstractBase {
public:
    virtual void pureVirtualFunction() = 0; // 纯虚函数
};

class ConcreteDerived : public AbstractBase {
public:
    void pureVirtualFunction() override {
        std::cout << "Implemented pure virtual function" << std::endl;
    }
};
```

### 5. 使用智能指针管理对象生命周期

使用智能指针（如`std::unique_ptr`和`std::shared_ptr`）可以更安全地管理对象生命周期，避免内存泄漏。

```cpp
int main() {
    std::unique_ptr<Base> ptr = std::make_unique<Derived>();
    ptr->display(); // 调用 Derived 的 display 方法
    
    // 演示信号和回调通知
    Base* rawPtr = new Derived();
    rawPtr->display();
    delete rawPtr; // 调用 Derived 和 Base 的析构函数

    return 0;
}
```

## C++面向对象|对象切片示例

```cpp
class Base {
public:
    Base(int val) : baseValue(val) {}
    virtual void display() const {
        std::cout << "Base display: " << baseValue << std::endl;
    }
protected:
    int baseValue;
};

class Derived : public Base {
public:
    Derived(int baseVal, int derivedVal) : Base(baseVal), derivedValue(derivedVal) {}
    void display() const override {
        std::cout << "Derived display: " << baseValue << ", " << derivedValue << std::endl;
    }
private:
    int derivedValue;
};
```

**对象切片发生**

```cpp
int main() {
    Derived d(1, 2);
    d.display(); // 输出: Derived display: 1, 2
    // 对象切片发生
    Base b = d;
    b.display(); // 输出: Base display: 1
    // 基类对象只包含基类部分，派生类的 derivedValue 被切掉
    return 0;
}
```

**避免对象切片,**应使用基类指针或引用来操作派生类对象

```cpp
int main() {
    Derived d(1, 2);
    d.display(); // 输出: Derived display: 1, 2

    // 1.使用基类指针
    Base* bp = &d;
    bp->display(); // 输出: Derived display: 1, 2

    // 2.使用基类引用
    Base& br = d;
    br.display(); // 输出: Derived display: 1, 2
    return 0;
}
```

## C++面向对象|虚函数

主要是用来实现多态。在基类函数前加上`virtual`关键字，在派生中`override`重写该函数，运行中根据对象实际类型调用对应函数。

```cpp
class Base {
public:
    virtual void show() {
        cout << "Base show" << endl;
    }
    virtual ~Base() {}  // 虚析构函数
};

class Derived : public Base {
public:
    void show() override {
        cout << "Derived show" << endl;
    }
};
```

### 虚函数表

- 当一个类包含虚函数时，编译器会为该类生成一个**虚函数表（vtable，即在编译期间创建）**，保存该类中函数的地址（32位：4字节，64位：8字节）；
- **虚函数表**位于只读数据段==(常量区|.rodata)==。虚函数存在于代码段==(代码区|.text)==；
- 同样，**派生类继承基类**，自然也会有虚函数，编译器也会为派生类生成自己的虚函数表（vtable）；

### 虚函数指针

- 当编译器检测到定义的派生类有虚函数，会为派生类对象**生成一个虚函数指针（vptr）**，指向该类型的虚函数表，这个虚函数指针的初始化是在构造函数中完成的；
- ==**vptr跟着对象走**==，所以对象什么时候创建，vptr就什么时候创建出来，所以是在程序运行时创建。如果对象在栈上创建，vptr 也在栈上；如果对象在堆上创建，vptr 也在堆上。 
- 如果有⼀个**基类类型的指针指向派生类**，那么当调⽤虚函数时，就会根据所指真正对象的**虚函数表指针**去寻找**虚函数的地址**，也就可以调⽤派⽣类的虚函数表中的虚函数**以此实现多态**。

- **编译时行为**

  - **虚函数表的生成**：在编译时，编译器为每个包含虚函数的类生成一个虚函数表。虚函数表存储在静态内存区域，因为它们的大小和内容在编译时已经确定，并且在程序运行期间不会改变。

  - **代码区**：虚函数的具体实现代码存储在代码区（通常是只读的）。

- **运行时行为**

  - **对象的创建**：在运行时，当创建一个包含虚函数的类的对象时，编译器在对象内添加一个虚函数表指针（vptr），并将其初始化为指向该类的虚函数表。

  - **对象在栈上**：如果对象在栈上分配，vptr 也在栈上。

  - **对象在堆上**：如果对象在堆上分配，vptr 也在堆上。

  - **对象在全局/静态区**：如果对象是全局或静态分配，vptr 也在全局/静态区。

## C++面向对象|虚函数优势劣势

**优势：**实现了多态，可以使相同的函数实现不同的功能，提高代码的复用性和接口的规范化。更加符合面向对象的设计理念

**劣势：**

- **运行时开销：**虚函数的动态绑定需要在运行时进行额外查找和解析，因此相对于非虚函数，有一定的运行时开销，导致性能稍微下降。
- **内存开销：**每个虚函数的类都会引入一个虚函数表指针，以及虚函数表。增加了内存开销，尤其在大规模的基层结构中
- 增加了设计的复杂性
- **缺少内联以及其他的优化机会：**编译器在编译阶段无法确定实际调用的函数版本，从而无法进行函数内联优化。并且限制编译器其他方面的优化能力。
- **缓存不友好：**虚函数的调用涉及**动态绑定**，但是动态绑定需要在运行时根据对象的实际类型进行函数调用。这种绑定导致了缓存不友好的情况。由于虚函数表存储了不同函数的地址，但是**不同对象又有不同的虚函数表指针**，这会导**致虚函数调用所涉及代码和数据分散在不同的内存位置**，导致**缓存失效和不连续内存访问**，从而降低缓存的命中率。


## C++面向对象|析构函数写成虚函数原因

如果⼀个类**可能被继承**，且在其**派生类**中有**可能使用 delete** 运算符来**删除通过基类指针指向的对象**，那么该基类的析构函数应该声明为虚析构函数。  

**防止内存泄漏**。若定义一个基类的指针指向一个派生类对象，在使用完毕准备销毁时，如果基类的析构函数没有被定义成虚函数，则**编译器根据指针类型就会认为当前的对象是基类，调用基类的析构函数**，进执行基类的析构，造成内存泄漏。

如果基类的析构函数被定义成虚函数，那么编译器就**根据实际对象，执行派生类的析构函数，再执行基类的析构函数，成功释放内存**。

## C++面向对象|构造函数为什么不能声明为虚函数

1. **对象类型确定性**：创建一个对象时需要确定对象的类型，而虚函数是在运行时确定类型的。而在构造对象时候，对象还没被创建成功，编译器无法知道对象的实际类型是类本身还是类的派生类；
2. **虚函数表的依赖性**：**虚函数的调用基于<u>虚函数表的虚函数指针</u>**，而该指针存放在**对象的内存空间**中。构造函数声明为虚函数，那么由于对象还没被创建，还没有对象的内存空间，也就没有虚函数表地址用来调用虚函数。

## C++面向对象|构造函数或析构函数中调用虚函数会怎样  

构造函数中调用虚函数会调用基类的版本，因为派生类版本尚未构造完成。这意味着派生类特有的实现不会生效，即使是在派生类的构造函数中

析构函数中调用虚函数同样会调用基类的版本，因为派生类版本已经被销毁。这通常不是问题，因为析构函数的目的是清理资源，通常不需要派生类特定的行为

语法通过，但是失去多态性。如果在构造或析构函数中调用虚函数，<u>会先**调用父类中的实现** 。</u>

父类中的实现：**构造函数**的执行顺序是**从基类构造函数开始**的。**析构函数**的执行顺序是**从派生类析构函数开始**的，调用基类析构函数时，**派生类**的部分**已经被析构**。

## C++面向对象|虚函数和纯虚函数的区别

有具体实现和无具体实现的区别

- 纯虚函数在基类中没有实现，必须在**派生类中实现**。纯虚函数用于定义抽象类，**抽象类不能被实例化，只能被继承。**

 ```cpp
  class AbstractBase {
  public:
      virtual void display() const = 0; // 纯虚函数
      virtual ~AbstractBase() = default; // 确保基类有虚析构函数
  };
  
  class ConcreteDerived : public AbstractBase {
  public:
      void display() const override { // 实现纯虚函数
          std::cout << "ConcreteDerived display" << std::endl;
      }
  };
 ```

## C++面向对象|有几种继承

无论何种继承，基类的私有成员在派生类中都是不可被访问的。只有通过基类的成员函数访问基类的私有数据成员

- **公有继承：**公有继承是最常见继承方式。在公有继承中，基类的公有成员和保护成员都会成为派生类的公有成员和保护成员。派生类是基类的一种类型，可以访问公有成员。**保护成员的访问权限是受限的，是通过基类类型的引用或指针实现的。**

- **私有继承：**基类的公有成员和保护成员都成为派生类的私有成员，无法直接访问。私有继承常用于实现继承的代码重用，而不是通过派生类对象访问积累的成员

- **受保护继承：**介于上面两者之间。在受保护继承中，积累的公有成员和保护成员成为了派生类的受保护成员，无法通过派生类对象访问。很少使用

- **虚继承：**用于解决多继承中的**菱形继承问题**（两个或多个派生类从同一个基类继承，然后一个最派生类从这两个派生类继承，对于子类的继承数量没有具体限制）。**虚拟继承通过使共同基类在继承链中只有⼀份实例**  

  ```cpp
  class Base {
  public:
      int baseVar;
  };
  
  // 错误
  class A : public Base {
  public:
      int aVar;
  };
  class B : public Base {
  public:
      int bVar;
  };
  
  // 正确
  class A : virtual public Base {
  public:
      int aVar;
  };
  class B : virtual public Base {
  public:
      int bVar;
  };
  
  // 多继承
  class E : public A, public B {
  public:
      int eVar;
  };
  
  int main() {
      E e;
      e.baseVar = 10; // 正确：没有歧义 错误：ambiguous call
      return 0;
  }
  ```


## C++内存管理|内存分区

- **运行时确定**

**栈**：自动管理、快速、空间有限。

**堆**：手动管理、灵活、空间大、易碎片化。

- **编译时确定**

**全局/静态区**：生命周期长、系统管理、占用内存大。

**常量区**：只读、安全、系统管理。

**代码区**：存储程序代码、只读、安全、系统管理。

![C++内存分区](D:\Yang7hi\note\typora_photo\InterviewBook\image-20240711162436798.png)

## C++内存管理|堆和栈的区别

1. **申请方式** 
   栈内存由操作系统自动管理，⽣命周期与其所在函数的执⾏周期相同；
   堆内存需要通过 `new` 和 `delete`（或 `malloc` 和 `free`）手动分配和释放，⽣命周期由程序员显式控制

2. **申请大小限制** 
   栈的大小在启动时由系统或编译器预先设定，通常较小。可以通过**`ulimit -a`**查看，**`ulimit -s`**修改。  内存是从高地址向低地址扩展的；
   堆内存可以在程序运行时动态调整大小，通常比栈大。分配内存块是不连续的内存区域，向高地址扩展。

3. **申请效率**
   栈由系统分配，速度快，不会有碎⽚；

   堆由程序员分配，速度慢，会有内存碎⽚

## C++内存管理|内存泄漏

内存泄漏是指由于疏忽或错误造成程序==**未能释放掉不再使用的内存的情况**==。「1.指针指向改变，2.未释放动态分配内存  」

内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，失去了对该段内存的控制，因而造成了内存的浪费。

可以使用`Valgrind，mtrace`进行内存泄漏检查

### 内存泄漏的分类

1. 堆内存泄漏

   程序运行中需要分配通过`malloc，realloc，new`等从堆中分配的一块内存，再是完成后必须通过对应的`free `或 `delete `删掉。如果程序的设计错误导致这部分内存没有被释放，那么此后这块内存将不会被使用，就会Heap leak。

2. 系统资源泄漏

   程序使用系统分配的资源，比如`Bitmap，handle，socket`等没有使用相应的函数释放掉，导致系统系统资源的浪费，严重导致系统效能降低，系统运行不稳定。

3. 没有将基类的析构函数定义为虚函数

   当基类指针指向子类对象时，如果基类的析构函数不是`virtual`，那么子类的析构函数将不会被调用，子类的资源没有被正确释放，因此导致内存泄漏

### 防止内存泄漏

将内存的分配封装在类中，构造函数分配内存，析构函数释放内存；使用智能指针

解决：加入错误处理代码，使用内存分析工具

## C++内存管理|常见内存泄漏

内存泄漏通常由以下情况引起：

1. 在类的构造和析构函数中没有匹配的调用 `new` 和 `delete`

   **描述**：如果在类的构造函数中分配了内存，但在析构函数中没有释放它，或者在析构函数中释放了与构造函数分配的内存不匹配的内存，就会导致内存泄漏或未定义行为。

2. 没有正确的清除嵌套的对象指针

   **描述**：如果一个对象的成员变量是指向另一个对象的指针，但在析构函数中没有正确释放这些嵌套对象，会导致内存泄漏。

3. 在释放对象数组时在 `delete` 中没有使用方括号

   **描述**：对于动态分配的对象数组，使用 `delete` 而不是 `delete[]` 释放内存，会导致未定义行为和内存泄漏。

   ```c++
   class MyClass {
   public:
       MyClass() : data(new int[100]) {}
       ~MyClass() {
           delete data; // 错误，应该使用 delete[]
           delete[] data; // 正确
       }
   private:
       int* data;
   };
   ```

4. 释放指向对象的指针数组时只释放了对象空间，没有释放指针空间

   **描述**：如果数组中存放的是指向对象的指针，在释放这些指针时，应该释放每个对象的内存和指针数组的内存。指针数组 `array` 没有被释放。

    ```cpp
    int main() {
        MyClass* array[10];
        for (int i = 0; i < 10; ++i) {
            array[i] = new MyClass();
        }

        for (int i = 0; i < 10; ++i) {
            delete array[i]; // 正确释放每个对象
        }
        // 忘记释放指针数组本身
    }
    ```
**解决方法**：释放每个对象的内存，然后释放指针数组本身。

5. 缺少拷贝构造函数和重载赋值运算符

   **描述**：默认的拷贝构造函数和赋值运算符执行浅拷贝，当类的成员变量是指针时，两个对象会指向同一块内存。释放一个对象的内存时，另一个对象也会尝试释放相同的内存，导致双重释放。

   ```c++
   int main() {
       MyClass a;
       MyClass b = a; // 调用浅拷贝构造函数，a 和 b 的 data 指向相同的内存
   }
   ```

   **问题**：`a` 和 `b` 的 `data` 指向同一块内存，双重释放会导致未定义行为。

   **解决方法**：定义拷贝构造函数和赋值运算符，执行深拷贝，或禁止拷贝操作。

6. 没有将基类的析构函数定义为虚函数

   **描述**：当基类指针指向派生类对象时，如果基类的析构函数不是虚函数，派生类的析构函数不会被调用，导致派生类的资源未被释放。

   **问题**：`Derived` 的析构函数没有被调用，导致资源泄漏。

   **解决方法**：将基类的析构函数声明为 `virtual`。

## C++内存管理|对`new`和`malloc`的理解

new 和 malloc都是动态内存分配函数。其中，new是C++的操作符号，malloc是c语言中的函数。new会调用对象的构造函数，malloc不会。使用new可以简化代码，并且类型安全

**区别**

1. **分配内存的位置：**malloc是从堆上动态分配内存，new是从自由存储区从对象动态分配内存。

   自由存储区的位置取决于operator new的实现。自由存储区不仅可以是堆，还可以是静态存储区，这都看operator new在哪里为对象分配内存。基本上，所有的C++编译器默认使用堆来实现⾃由存储  

2. **返回类型安全性**：malloc内存分配成功后返回void*，然后再强制类型转换为需要的类型；new操作符分配内存成功后返回与对象类型相匹配的指针类型。因此new是符合类型安全的操作符；

3. **内存分配失败返回值：**

   malloc内存失败返回NULL，

   new失败会抛异常（std::bac_alloc),如果加上std::nothrow关键字,就不会抛出异常⽽是会返回空指针;

4. **分配内存的大小计算：**使用new操作符申请内存分配时无须执行内存块的大小，编译器会根据类型信息进行计算，而malloc则需要显式地指出所需内存尺寸大小；

5. **是否可以被重载：**operator new/operator delete可以被重载，而malloc/free则不能重载。

## C++内存管理|delete和free理解

1. **类型安全性:**

   delete会调用对象的析构函数，确保资源被正确释放

   free不了解对象的析构和构造，只是简单释放内存块；

2. **内存块释放后行为：**

   delete释放的内存块的指针会被设置为nullptr，避免野指针

   free不会修改指针的值，可能导致野指针问题

3. **数组的释放**

   delete可以正确释放通过new分配的数组

   free不了解数组大小，不适合用于释放通过malloc分配的数组

## C++内存管理|malloc底层是怎么分配内存的



## C++内存管理|野指针 悬空指针

```cpp
/**
*野指针指的是没有被初始化过的指针。
*为了防⽌出错，对于指针初始化应该要赋值为NULL
*/
int main() {
    int *wildPtr;
    // 野指针：没有初始化的指针
    cout << "Value pointed by wildPtr: " << *wildPtr << endl; // 未定义行为
    
    int *ptr = new int; // 动态分配内存
    *ptr = 10; // 初始值
    cout << "Value pointed by ptr: " << *ptr << endl;
    delete ptr; // 释放内存：指针最初指向的内存已经被释放了的⼀种指针
    // 此时ptr成为悬空指针，但是以下代码错误地使用了这个悬空指针
    cout << "Value pointed by danglingPtr: " << *ptr << endl;
    
    return 0;
}
```

### 如何避免野指针

- 在释放内存后将指针置为nullptr
- 避免返回局部变量的指针
- 使用智能指针（unique_ptr和shared_ptr)
- 注意函数生命周期



## C++内存管理|`strlen`和`sizeof`的区别 

  1. **`sizeof`** 是**运算符**，不是函数
     **编译时运算符**，用于确定数据类型或变量的大小（以字节为单位）。它在编译时计算，所以不会对程序的运行时间产生影响。由于sizeof在编译时确定大小，它不能用于计算动态分配内存的大小。**`malloc` 和 `free`**：用于动态内存分配和释放。
  2.  **`strlen`** 是字符处理库函数
     **库函数**，用于计算以 `'\0'` 结尾的C风格字符串的长度。它在运行时通过遍历字符串来计算长度，不包括终止符 `'\0'`。

## C++ STL|栈和队列的区别

1. 队列**FIFO**先进先出，栈**LIFO**后进先出

2. **插入和删除操作的限制**
   栈：只能在表的一端（栈顶）进行插入和删除操作
   队列：只能在表的一端（队尾）进行插入操作，在另一端（队首）进行删除操作。

3. **遍历数据速度和方式**
   栈：只能从栈顶逐个取出数据，最先放入的元素需要遍历整个栈最后才能取出来。遍历时还需要**`额外的临时空间`**，保持数据在遍历前的一致性。
   队列：队首取出数据再放入队尾的方式来遍历数据，不需要额外空间。

## C++ STL|vector的实现

- **底层实现**：vector在堆中分配了⼀段连续的内存空间来存放元素 
- **三个迭代器**
  - **`first:`**指向的是vector中对象的起始字节位置
  - **`last:`**指向的是vector中对象的末尾字节位置  
  - **`end:`**指向的是vector中所占内存空间的末尾字节位置
    ![C++vector容器-构造函数_c++ vector动态扩容 构造函数-CSDN博客](D:\Yang7hi\note\typora_photo\InterviewBook\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzUxOTU1NDcw,size_16,color_FFFFFF,t_70#pic_center.png)

`rend()` 函数用于返回指向 `vector` 末尾的逆向迭代器。逆向迭代器允许你以逆向顺序遍历 `vector` 中的元素。

## C++ STL|vector的扩容过程

1. 当 `vector` 满时（即当前元素个数达到容量），插入新元素会触发扩容。
2. **分配更大内存**：`vector` 会分配一块新的、更大的内存空间，**通常是原容量的两倍。**
3. **复制数据**：将现有数据从旧内存复制到新内存。
4. **释放旧内存**：释放掉原来的内存空间。
5. **插入新元素**：在新内存中插入新增的元素。
6. **注意**：由于内存重新分配，**所有指向原 `vector` 的迭代器、引用和指针都会失效**。

### vector|`size()`与`capacity()`

- **`size()`**

  - 返回 `vector` 当前存储的元素个数，即实际元素的数量。

  - `size()` 是一个**动态变化**的值，随着元素的添加或删除而改变。

- **`capacity()`**

  - 返回 `vector` 在不需要重新分配内存的情况下，最多可以容纳的元素个数。

  - `capacity()` 表示分配的内存空间的总容量，是一个**静态**值，只有在扩容时才会变化。

- **扩容条件**：当 `size` 等于 `capacity` 时，`vector` 扩容，增加 `capacity`，通常是将其翻倍，从而腾出更多的空间用于存储新元素。

### vector|扩容方式

- **固定扩容 Fixed Increment Growth**：空间利用率高；但是多次扩容可能事件复杂度高

- **加倍扩容 Doubling Growth**：需要扩容的次数大大减少（预留空间较多），时间复杂度较低；但是可能很多空间没有利用上，空间利用率不高

- **`resize()` 和 `reserve()`**

  - resize()：改变当前容器内含有元素的数量size()，而不是容器的容量。指定n，精确控制容器中的元素数量。
    - 如果n < size，则将元素size减少到前n个，移除多余的元素(并销毁），capacity不变;  
    - 如果n > size，则在容器中追加元素，如果val指定了，则追加的元素为val的拷贝，否则，默认初始化
    - 如果n <= capacity，只调整 `size`，`capacity` 不变。
    - 如果n > capacity，内存会自动重新分配
  
  -  reserve()：改变当前容器的最大容量（capacity）。指定n，避免频繁扩容。
    - 如果n > 容器的当前capacity，该函数会使得容器重新分配内存使capacity达到n
    - 当n <= 当前的capacity()，则数组中的capacity不变， size不变，即不对容器做任何改变  
  
  
  ```cpp
  void resize(size_type n, value_type val = value_type());
  void reserve(size_type n)
  ```

## C++ STL|vector的缺点

**插入和删除效率低**：在头部或中间进行插入或删除操作需要移动大量元素，性能较差。

**动态添加数据的重新分配**：内存重新分配会带来性能开销，并导致迭代器失效。

**访问元素时没有边界检查**：使用下标访问元素时不进行边界检查，可能导致安全问题。

**不支持多维数组**：需要通过嵌套 `vector` 来实现多维数组，增加了复杂性和可能的性能问题

这些缺点使得 `vector` 在某些特定的应用场景下可能不是最佳选择。例如，对于频繁插入和删除操作的场景，可能需要考虑其他容器（如 `deque` 或 `list`）。

## C++ STL|deque（双端数组）  

- **存储结构**
  - 和**`vector`**容器采用连续的线性空间不同，**`deque`**容器存储数据的空间是由一段一段等长的连续空间构成（**部分连续**），各段空间之间并不一定是连续的，可以位于在内存的不同区域。
  - **支持快速随机访问**，由于`deque`需要处理**内部跳转**，因此速度没有vector快。

- **连续空间内部管理**

  - **`deque`维护一个 map数组作为主控**，管理这些连续块。这些指针指向在内存中不连续的各个数据块。
  - 数据块的实际存储是在这些块的**缓存区（buffer）**中进行的。

- **设计：**⼀旦有必要在其头端或者尾端增加新的空间，便配置⼀段定量连续空间，串接在整个deque的头端或者尾端  

  - **好处：**避免「vector的重新配置，复制，释放」的轮回，维护**整体连续的假象**，并提供随机访问的接口

  - **坏处：**其迭代器变得相当复杂，需要内部跳转和多个数据块。

- **数据结构**

  - deque 还需要维护 **start、finish 这 2个deque 迭代器**，分别指向 `deque` 的头端和尾端数据块。
  - **由`deque:begin()`与`deque:end()`传回。**start 迭代器记录着 map 数组中首个连续空间的信息，finish 迭代器记录着 map 数组中最后一个连续空间的信息。
  - `deque`必须记住`map`大小，以便正确管理


***tip：**`deque`迭代器较`vector`复杂，为了提升效率，在`deque`进行排序操作时候，可以先把`deque`复制到`vector`中再进行排序最后再返回deque*

- **总结**

在 `deque` 容器中，**`map` 数组**作为中控器，管理着所有数据块的指针，这些指针指向实际存储数据的**`buffer`**。每个 `buffer` 是一个在内存中不连续的数据块，用于存储 `deque` 的元素。**`迭代器`** 通过 `map` 数组定位到具体的 `buffer`，从而实现对 `deque` 元素的访问和操作。`map` 数组协调这些不连续的 `buffer`，使得 `deque` 能在头尾动态扩展的同时保持高效的随机访问能力。

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\58eff65eeb8dd7687924494a74d00f4c.png" style="zoom:50%;" />

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\2-19121316430U40.gif" alt="img" style="zoom:50%;" />

  ```cpp
  template <class T, class Alloc = std::allocator<T>, size_t BufSize = 0>
  class deque {
  public:
      typedef T value_type;
      typedef value_type* pointer;
      typedef size_t size_type;
  
      // Iterator class
      class iterator {
  	// ...
      };
  
  protected:
      typedef pointer* map_pointer;
      
      iterator start;
      iterator finish;
      map_pointer map; 	// 指向map
      size_type map_size; // map可以保存的指针数
  
  	// 初始化map和buffer的辅助函数
      void initialize_map() {
          // 假设这里的地图大小和分配逻辑…
      }
  
  };
  // T：元素类型
  // Alloc：分配器类型，默认为std::allocator
  ```

## C++ STL|`deque`&`vector`的区别

|          |                           `deque`                            |                           `vector`                           |
| -------- | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 内存结构 |                  分段连续内存块（链式结构）                  |                   动态数组，单一连续内存块                   |
| 复杂度   | **随机访问：** O(1)；**插入删除：**在头部和尾部 O(1)；在中间 O(n) | **随机访问：** O(1)；**插入删除：**在尾部 O(1)；在头部或中间 O(n) |
| 组织方式 |        按页或块来分配存储器的，每页包含固定数目的元素        |                 分配一段连续的内存来存储内容                 |
| 效率     | 即使在容器的前端也可以提供常数事件的`insert`和`erase`操作，而且在体积增长方面也比`vector`更具有效率 | 只是在序列的尾端插入元素时才有效率，但是随机访问速度要比`deque`快 |

## C++ STL|`stack`&`queue`的区别

`stack` 是一个后进先出（LIFO）的数据结构。`queue`是一个先进先出（FIFO）的数据结构。栈和队列被称之为**deque的配接器**，其底层是以deque为底部架构的，通过deque执行具体操作。

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\b03be4eacc15474a9d995d37c5381a22.png" alt="img" style="zoom: 50%;" />

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\6bbbe7faf59740b99ecc987932d892b6.png" alt="img" style="zoom:50%;" />

## C++ STL|`map` & `unordered_map`的区别

|            map             |        unordered_map        |
| :------------------------: | :-------------------------: |
|            有序            |            无序             |
|           红黑树           |           哈希表            |
|      O(logn) 「稳定」      |   O(1) 「平均。最快O(n)」   |
| map中元素是⼀些key-value对 | 以（key,value）对的形式存储 |

## C++ STL|vector，list，map，unordered_map各自特点和原理

|                            vector                            |                             list                             |              map              |       unordered_map        |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :---------------------------: | :------------------------: |
|      底层通过数组实现，存储空间上⼀段**连续的内存空间**      | 通过双向链表实现，把**不连续的内存空间**通过链表的方式连接在一起 |         底层是红黑树          |        底层是哈希表        |
|   插入删除时间复杂度为O(N) ；支持随机访问，时间复杂度O(1)    | 插入删除时间复杂度为O(1) ；不支持随机访问，需要遍历整个链表时间复杂度O(N) | 增删、查找时间复杂度为O(logn) | 增删、查找时间复杂度为O(1) |
| 空间不足时需要另开辟**⼀个俩倍于当前空间⼤小的空间**，然后将原有的元素复制过去，再析构原空间，会造成原有的迭代器失效 |  每次插入和删除的时候分配和释放空间，所以不会引起迭代器失效  |             有序              |            无序            |

**Map为什么不用AVL树：**红黑树和AVL共同属于平衡二叉树。红黑树确保没有⼀条路径会比其他路径长出两倍，因此是⼀种弱平衡树。AVL树要求严格和平衡，需要更加频繁的旋转来维护） 

## C++ STL | 迭代器有什么作用？什么时候迭代器会失效  

迭代器为不同类型的容器提供了统一的访问接口，隐藏了底层容器的具体实现细节，允许开发者使用一致的语法来操作不同类型的容器。

- 对于序列容器vector，deque来说，使用erase后，后边的每个元素的迭代器都会失效，后边每个元素的迭代器都会失效，后边每个元素都往前移动一位，erase返回下一个有效的迭代器
- 对于关联容器map，set来说，使用了erase后，当前元素的迭代器失效，但是其结构是红黑树，删除当前元素，不会影响下一个元素的迭代器，所以在调用erase之前，记录下一个元素的迭代器即可
- 对于list而言，它使用了不连续分配的内存，并且它的erase方法也会返回下一个有效的迭代器，因此上面两种方法都适用

## C++新特性 智能指针

智能指针作用是用来管理一个指针 防止程序员申请的空间在函数结束时没有被释放，从而造成`内存泄漏`的发生。

使用智能指针可以很大程度避免这个问题，`因为指针指针是一个类`，当超出类的作用域范围时，会自动调用类的析构函数，从而释放资源。

**四种指针指针**：auto_ptr，unique_ptr，shared_ptr，weak_ptr

## C++新特性 智能指针 | 什么场景下使用

动态内存管理中，使用智能指针可以避免手动管理内存的麻烦和出错风险

## C++新特性 智能指针 |  `auto_ptr` 

`auto_ptr`采⽤所有权的模式，在c++11中被废弃了  

- **`auto_ptr`在拷贝和赋值操作时会导致所有权转移**。`auto_ptr`以copy的语义来转移指针资源，转移指针资源的所有权的同时，会将**`原指针置为NULL`**  (一般的copy，原资源不会更改)
- 使用容器保存`auto_ptr`后，在进行操作后，可能导致容器中保存的原`auto_ptr`所管理的对象失效

## C++新特性 智能指针 | `shared_ptr  `

**共享智能指针。**实现共享式拥有的概念，多个智能指针可以同时**指向相同的内存资源**。

可以调用`use_cont()`来查看资源的**所有者个数**，并调用`release() `来释放当前指针的的资源所有权，使计数减1，为0时资源会被释放。可能会产生**循环引用**。

> **循环引⽤**：两个对象相互使⽤`shared_ptr`指向对⽅  

## C++新特性 智能指针 | `weak_ptr   `

**弱引用指针** 。指向`shared_ptr`管理的对象，解决**循环引用**的问题.

只可以从⼀个`shared_ptr`或另⼀个`weak_ptr`对象构造，构造和析构不会引起引⽤计数的增加或减少 

## C++新特性 智能指针 | `unique_ptr `

**`独占所有权特性`**。同⼀时刻只能有⼀个`unique_ptr`指向给定对象，离开作⽤域时，若其指向对象，则将其所指对象销毁（默认delete）。 

`unique_ptr` 不支持普通的拷贝和赋值操作，支持移动构造和移动赋值操作，这意味着所有权可以从一个 `unique_ptr` 转移到另一个 `unique_ptr`。

```cpp
unique_ptr<int> ptr1 = make_unique<int>(42);
cout << "ptr1: " << *ptr1 << endl;

// 移动构造，将所有权从 ptr1 转移到 ptr2
unique_ptr<int> ptr2 = move(ptr1);
cout << "ptr2: " << *ptr2 << endl;

// ptr1 现在为空，因为它的所有权已被转移
if (!ptr1)  cout << "ptr1 is now empty" << endl;
```

## C++新特性|lambda表达式

lambda表达式（匿名函数）代表一个可调用的代码单元，没有命名的内联函数，不需要函数名因为我们直接一次性用它，不需要其他地方调用。

1. **`lambda`**表达式语法

```cpp
auto lam = [captures] (parameters) -> return_type { body }
// [捕获列表] (参数列表) -> 返回类型 {函数体 }
// 只有 [capture list] 捕获列表和 {function body } 函数体是必选的
```

大多情况lambda表达式返回值由编译器猜测，不需要指定返回值类型。

2. **`lambda`**表达式特点

   使用STL中的算法(algorithms)库

   **变量捕获**

   - **不捕获**：使用空的捕获列表`[]`，Lambda表达式内部不能访问外部变量。
   
     ```cpp
     auto lam = []() { /* 这里不能访问外部变量 */ };
     ```

   - **引用捕获所有**：使用`[&]`，Lambda表达式可以引用外部作用域中的所有变量。
   
     ```cpp
     auto lam = [&]() { cout << "Value: " << someExternalVar; };
     ```

   - **值捕获所有**：使用`[=]`，Lambda表达式会复制外部作用域中的所有变量的值。
   
     ```cpp
     auto lam = [&]() mutable { someExternalVar++; }; // mutable允许修改捕获的变量
     ```

    - **混合捕获**：使用`[=, &foo]`，Lambda表达式以引用方式捕获变量`foo`，其余变量以值的方式捕获。
   
      ```cpp
      auto lam = [=, &foo]() { /* 使用值捕获其他变量，引用捕获foo */ };
      auto lam = [&]() { /* 使用引用捕获其他变量，值捕获foo */ };
      ```
      
    - **单个变量值捕获**：使用`[bar]`，Lambda表达式只捕获变量`bar`的值，不捕获其他变量。
   
      ```cpp
      auto lam = [bar]() { /* 只捕获bar的值 */ };
      ```
   
    - **捕获this指针**：使用`[this]`，Lambda表达式捕获其所在类的`this`指针。
   
      ```cpp
      class MyClass {
      public:
          void myMethod() {
              auto lam = [this]() { /* 使用this指针 */ };
          }
      };
      ```
   
   

## C++新特性|右值引用

**左值**:一个内存实体，有名字并且可以通过引用获取地址的实体，它们在程序执行过程中会长期存在。

**右值**:在表达式中短暂存在，没有独立命名的临时对象，使用`&&`操作符表示。它们通常作为表达式的结果存在，仅在表达式的求值过程中存在。右值不能通过`&`操作符获取地址，因为它们是临时的，使用一次就消失。

- **`std::move`**是一个标准库函数模板，**用于将左值转换为右值引用**。

  它本质上并不移动任何东西，只是将左值的身份转换为右值引用，以便在需要时使用右值引用进行移动语义。

  ```cpp
  int main() {
      MyClass a(10);               // 创建一个对象
      MyClass b = std::move(a);    // 通过移动构造函数转移资源
      MyClass c;
      c = std::move(b);            // 通过移动赋值运算符转移资源
      return 0;
  }
  ```

实现一个类的时候，会在 **移动构造函数** 和 **移动赋值运算符** 中加以应用右值引用

- 移动构造函数

  ```cpp
  class MyClass {
  private:
      int* data;
  
  public:
      // 默认构造函数
      MyClass() : data(nullptr) {}
  
      // 带参构造函数
      MyClass(size_t size) : data(new int[size]) {}
  
      // 移动构造函数
      MyClass(MyClass&& other) noexcept : data(nullptr) {
          data = other.data;       // 转移资源
          other.data = nullptr;    // 置空源对象的资源指针
      }
  
      // 析构函数
      ~MyClass() {
          delete[] data;
      }
  };
  ```

- 移动赋值运算符

  ```cpp
  class MyClass {
      // 类的其他成员...
  
  public:
      // 移动赋值运算符
      MyClass& operator=(MyClass&& other) noexcept {
          if (this != &other) { // 检查自赋值
              delete[] data;    // 释放当前对象的资源
              data = other.data;       // 转移资源
              other.data = nullptr;    // 置空源对象的资源指针
          }
          return *this;
      }
  };
  ```

### 右值引用作用

1. 可以通过右值引用来实现**移动语义**。移动语义可以在**不进行深拷贝**条件下，将对象资源的所有权从一个对象转到另一个对象，从而**提高代码效率**；

   移动语义仅针对于那些实现了移动构造函数的类的对象，对于那种基本类型int、float等没有任何优化作⽤，还是会拷⻉，因为它们实现没有对应的移动构造函数

   ```c++
   std::vector<int> source = {1, 2, 3, 4, 5};
   std::vector<int> destination = std::move(source);
   ```

2. 右值引用可以用于**完美转发**。在函数模板template中，通过使用右值引用的形参来接受参数，可以实现完美转发（**即保持原参数的值类别，左值或右值**），将参数传递给另一个函数。

   通常，`std::forward` 与右值引用（`T&&`）一起使用，在模板函数中转发参数。这里的 `T` 是一个模板参数，表示传递给模板函数的参数类型。

   ```cpp
   // 一个通用函数，能够接收任意类型的参数
   template <typename T>
   void forwardFunction(T&& arg) {
       std::cout << "forwardFunction called" << std::endl;
       anotherFunction(std::forward<T>(arg)); // 使用std::forward进行完美转发
   }
   
   // 定义两个重载的目标函数，分别接受左值引用和右值引用
   void anotherFunction(int& arg) {
       std::cout << "Lvalue reference called with value: " << arg << std::endl;
   }
   
   void anotherFunction(int&& arg) {
       std::cout << "Rvalue reference called with value: " << arg << std::endl;
   }
   
   int main() {
       int x = 10;
       std::cout << "Passing lvalue:" << std::endl;
       forwardFunction(x);         // 传递左值，调用接受左值引用的目标函数
   
       std::cout << "Passing rvalue:" << std::endl;
       forwardFunction(20);        // 传递右值，调用接受右值引用的目标函数
   
       return 0;
   }
   ```


## C++新特性|`emplace_back`函数

**在容器的末尾就地构造一个新元素。**接收构造新元素所需的参数，然后直接在容器的内存空间中构造这个元素；

与**`push_back`**不同的是，不需要创建**临时对象**，再将其复制或移动到容器中，而是直接在容器中构造新的元素。因此通常更为高效，避免创建和销毁开销

emplace_back 的参数是传递给元素类型的构造函数的参数：提供的参数会被直接传递给容器中元素类型的构造函数，以便在容器的内存空间内就地构造一个新的元素实例。

```c++
void emplace_back(Args&&... args);
```

## C++函数调用过程

函数调用的过程不仅仅是控制权转移，还涉及到复杂的栈、寄存器和内存管理。

1. **函数调用前的准备**：
   - 编译器生成代码准备函数调用，包括将参数压栈和设置返回地址。
   - 调用者保存必要寄存器的值，以符合调用约定。
2. **函数调用栈帧的创建**：
   - 每次函数调用时，会在栈上创建一个新的栈帧，用于存储局部变量、返回地址和其他必要信息。
   - `ebp`（基指针）通常指向当前函数栈帧的开始地址，而`esp`（堆栈指针）指向栈帧的最后一个地址。
3. **寄存器管理**：
   - 调用者保存（Caller-save）寄存器需要在函数调用前由调用者保存。
   - 被调用者保存（Callee-save）寄存器需要在函数调用中由被调用者保存和恢复。
4. **执行过程**：
   - 函数入口：保存返回地址，调整栈帧，保存寄存器。
   - 函数执行：访问参数和局部变量，执行函数逻辑。
   - 函数退出：处理返回值，调整栈帧，恢复寄存器，返回到调用点。
5. **控制权返回**：
   - 函数执行完毕后，控制权返回到调用者，调用者继续执行后续代码。

[:link:CPU阿甘：函数调用的秘密](https://mp.weixin.qq.com/s/EoZyMgjEml_2rWu1dA85dA?)

## C++和python的区别

- c++是⼀种编译型语⾔，需要将代码编译成可执行⽂件才能运⾏；python是⼀种解释型语⾔，可以直接运行脚本；
- c++是⼀种静态类型语⾔，需要在编译时指定变量的数据类型；python是⼀种动态类型的语⾔，可以在运行时解析变量类型
- c++可以直接操作内存，效率⾼；python具有很⾼的开发效率，但性能较低  
  **相较于其他语言，有什么特点**
- 引入了模块概念，可复用性高
- 运行效率快
- 三大特性：继承 多态 和 封装

## 三种i=i+1执行效率比较

i = i + 1, i += 1 , i++。在实际编译过程中，编译器会自动优化，所以效率⼀样。

如果没有编译器优化：

1. i = i + 1 效率最低。先读取右i地址，将值 +1 --> 读取左i地址 --> 将右值传给左i；
2. i += 1 次之。读取i地址 --> 值 +1 --> 传给i；
3. i++ 效率最高。读取i地址 --> 自增 1

## `extern "C"`

⽀持 C++ 与 C 混合编程

告诉 C++ 编译器在链接指定的函数或变量中按照 C 语言的规则来进行，除函数重载外，**`extern "C"`**不影响其他特性

**应用场景**（兼容性和可移植性）

- **跨语言集成**：能够被其他不支持 C++ 名称修饰的编程语言调用
- **创建 DLL 或共享库**：避免因名称修饰导致外部程序无法正确解析函数名

## 情景题。手机店。不同品牌的不同型号手机有不同的业务逻辑。怎么设计系统

设计目标：能够灵活地处理不同品牌和型号的手机。同时保持代码的可理解性、可扩展性和可维护性。



---



# `计算机网络`

## HTTP|超文本传输协议，Hypertext Transfer Protocol

基于TCP/IP协议之上的应用层协议，基于请求-响应模型

## HTTP|HTTP报文

### HTTP请求报文

**其主要由请求行、请求头、请求体构成** 

1. **请求行（Request Line） | 请求方法URI协议版本号**

   - 请求方法：GET、POST、PUT、DELETE、PATCH、HEAD、OPTIONS、TRACE

   - **URI**：统一资源标识符，指定所请求的资源位置。<协议>://<主机>:<端口><路径>?<参数>
     - 区别URL：统⼀资源定位符， URL是URI的⼀种特殊形式，它不仅标识资源，还提供了资源的位置信息，即如何定位和获取资源。  

   - 协议版本号：HTTP版本号

2. **请求头（Request Headers）**

   包含请求的附加信息。由 `key : value ` 组成，它可以包含很多不同的字段，用于告知服务器有关请求的详细信息。

   **一些常见的==请求字段==包括：**

   - ==Host==：指定服务器的主机名和端口号

   - ==User-Agent==：表示客户端的用户代理

   - ==Accept==：指定客户端可以接受的响应的MIME类型

   - Connect-Type：指定请求主体的MIME类型

   - Authorization：用于进行身份验证的凭据
     ```http
     GET /api/v1/resource HTTP/1.1
     Host: www.example.com
     User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
     Accept: application/json, text/plain, */*
     Content-Type: application/json
     Authorization: Bearer <your_access_token>
     ```
     

3. **空行（Blank Line）**

   空行是请求头部和请求主体之间的空行，用于分割请求头部和请求主体

4. **请求体（Request Body）**

   承载多个请求参数的数据，请求主体是可选的，通常在发送POST、PUT等请求时包含请求的实际数据。

   <img src="D:\Yang7hi\note\typora_photo\InterviewBook\4.jpg" alt="HTTP 的消息格式" style="zoom: 50%;" />

### HTTP响应报文

响应报文是服务器向客户端返回的数据格式，用于传达服务器对客户端请求的处理结果以及相关的数据。包括状态行、响应头、响应体。

1. **状态行(Status Line)** 

   - HTTP协议版本：一般为HTTP/1.1
   - HTTP状态码 
   - 状态信息:对状态码的简要描述  

   ```http
   HTTP/1.1 200 OK
   ```

2. **响应头部(Response Headers)** 

   类似请求头，以键值对形式提供响应的附加信息。告知客户端有关请求的详细信息。

   **一些常见响应头部字段：**

   - Content-Type：指定响应主体的MIME类型
   - Content-Length:指定响应主体的长度（字节数
   - Server：指定服务器的信息
   - Location：在重定向时指定新的资源位置
   - Set-Cookie：在响应中设置Cookie

   ```http
   HTTP/1.1 200 OK
   Content-Type: text/html; charset=UTF-8
   Content-Length: 3145
   Server: nginx/1.18.0
   Location: http://www.example.com/redirected-page
   Set-Cookie: userSession=abc123; Max-Age=3600; Path=/; HttpOnly
   
   <html>
   <head>
       <title>Example Page</title>
   </head>
   <body>
       <h1>Welcome to Example.com</h1>
       <p>This is a sample page.</p>
   </body>
   </html>
   ```

3. 空行(Empty Line):分隔响应头部和响应主体  

4. **响应主体(Response body)**

   包含服务器返回给客户端的实际数据。例如，当请求⼀个网页时，响应主体将包含HTML内容。响应主体的存在与否取决于请求的性质以及服务器的处理结果。

> MIME（Multipurpose Internet Mail Extensions，多用途互联网邮件扩展）是一种标准，最初设计用于定义电子邮件消息的格式，包括消息的类型、编码方式以及消息的其它属性。MIME 使得电子邮件能够支持文本以外的多种数据格式，如图像、音频和视频等。

## HTTP|HTTP状态码

1. **1xx（信息性状态码）**：接收的请求正在处理。
- 100：继续。客户端继续其请求
  
- 101：切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议。例如，切换到HTTP的新版本


2.  **2xx（成功状态码）**：请求正常处理完毕。200、204、206
    - 200：客户端请求成功
    - 204：服务器成功处理了请求，无返回内容
    - 206：服务器成功处理了部分GET请求，服务器返回的body数据是资源的一部分

3.  **3xx（重定向状态码）**：表示需要后续操作以完成请求。301、302、304
    - 301：永久重定向
    - 302：临时重定向
    - 304：（未修改）上次请求后，请求网页没被修改

4.  **4xx（客户端错误状态码）**：400、403、404
    - 400：请求报⽂语法有误，服务器⽆法识别  
    - 401：请求需要认证
    - 403：请求的资源禁止被访问
    - 404：服务器无法找到对应资源

5.  **5xx（服务器错误状态码）**：500、501、502、503、504、505  
    - 500：服务器内部错误
    - 503：服务器正忙
    - 504：充当网关或代理的服务器，未及时从远程服务器获取请求
    - 505：服务器不支持请求的HTTP协议版本，无法完成处理


## HTTP|请求报文的`请求方法`

1. **GET：获取资源**，不对服务器产生影响（安全、幂等）
2. **POST：发送数据**，**例如提交表单数据、上传文件等，会影响服务器，服务器可能动态**创建新的资源（资源不存在时，区别PUT）或更新原有资源**（不安全、不幂等）
3. HEAD：类似GET，**仅要求服务器返回头部信息**，不返回实际的资源内容。 
4. PUT：用于更新服务器上的资源或创建**新资源**  
5. DELETE：请求服务器**删除指定的资源**。  
6. TRACE：**测试**。要求目标服务器返回原始的HTTP请求内容  
7. PATCH：对资源进行**部分更新**
8. CONNECT：用于代理服务器
9. OPTION：获取服务器支持的HTTP方法列表，以及针对指定资源支持的方法

- **安全：**请求方法不会破坏服务器资源
- **幂等：**多少次相同操作，结果不变

|          |                 GET                  |                          POST                          |
| :------: | :----------------------------------: | :----------------------------------------------------: |
|   基本   |        获取资源，不影响服务器        | 提交数据，影响服务器，可能动态创建新资源或更新原有资源 |
| 参数传递 | 写在URL中，数据量小，只接受ASCII字符 |        写在请求体中，无长度限制，无数据类型限制        |
| 协议版本 |               HTTP1.1                |                        HTTP1.1                         |
| 编码方式 |             只能URL编码              |                    支持多种编码方式                    |
| 安全幂等 |              安全、幂等              |                     不安全、不幂等                     |
| 时间消耗 |            一个TCP数据包             |  两个TCP数据包，先发送Header，响应，浏览器再发送data   |
| 缓存机制 |       可以被缓存，浏览器可回退       |                       不会被缓存                       |

## HTTP|HTTPS区别

- HTTP协议传输的数据都是未加密的，也就是明文传输，存在安全风险的问题。HTTPS在TCP和HTTP网络层之间加入了SSL/TLS安全协议，使得报文能够加密传输。
- HTTP连接较为简单，TCP三次握手之后就可以进行HTTP报文传输，而HTTPS在握手之后，还要进行SSL/TLS握手，才可以进行传输；
- 默认端口：http-80  https-443
- HTTPS需要向CA（证书权威机构）申请数字证书，来保证服务器的身份是可信的

## HTTPS|怎么保证安全的，保证的是哪部分安全  

HTTPS 在 HTTP 与 TCP 层之间加入了 SSL/TLS 协议，以确保传输的数据在⽹络上的安全性HTTPS保证了三个主要方面的安全

- 信息加密：HTTPS 使用**混合加密机制**，对称加密用于数据本身加密解密，非对称加密用于安全地交换密钥。确保第三方无法被解惑或窃听；
- 校验机制：HTTPS 使用**消息摘要算法**来确保数据的完整性，以检测数据是否在传输过程中被篡改；
  - 摘要算法：哈希函数，由内容推出唯一哈希值，哈希值无法反推内容。
- 身份认证：HTTPS 使用**数字证书**来验证服务器的身份。这些证书由受信任的第三方机构签发。

## TCP/IP四层模型 | OSI七层模型

OSI 模型是广泛用于教学和概念解释的标准模型。它详细描述了每一层的功能，因此在解释具体协议时，使用 OSI 模型能提供更清晰的分层概念。

![在这里插入图片描述](D:\Yang7hi\note\typora_photo\InterviewBook\118047e41cc042a4b71ee4d72dde365c.png)

- 应用层：主要为应用程序提供服务，工作在用户态，以下几层工作在内核态；
  - 应用层：负责给应用程序提供统一的接口：HTTP、FTP、DNS
  - 表示层：负责把数据转换到兼容一个系统的格式：LPP轻量级会话协议
  - 会话层：负责建立、管理和终止表示层实体之间的通信会话：LDAP轻型目录访问协议

- 传输层：包含**TCP和UDP协议**，负责建立、管理和维护**端到端的连接**；
- 网络层：负责IP的选择和路由的选择；ICMP
- 网络连接层：也可以分为数据链路层和物理层，主要为网络层提供链路级别的传输服务，负责在以太网、WIFI这样的底层玩咯上发送原始数据包。
  - 数据链路层：负责数据的封帧和差错检测，以及MAC寻址：ARP协议
  - 物理层：负责物理网络中的传输数据帧：Ethernet协议


## TCP|TCP三次握手

1. 一开始，客户端和服务端都属于`close`状态。先是服务端主动监听某个端口，处于`listen`状态

2. 客户端随机初始化一个序列号`clinet_isn`，将此序号至于TCP首部的`序号`字段中，同时把`SYN标志位`置为`1`，表示`SYN报文`。接着把第一个SYN报文发送给服务端，表示向服务端发送连接，该报文不包含应用层数据，之后客户端处于`SYN-SENT`状态；

3. 服务端收到客户端的`SYN报文`后，首先服务端也会随机初始化自己的序列`server_isn`，将此序列号填入首部的`序号`中，其次把TCP首部的确认应答号字段填入`clinet_isn+1`，接着把`SYN和ACK标志位`置为1，最后把报文发送给客户端，该报文不包含应用层数据。之后服务端处于`SYN-REVD`状态；

4. 客户端收到服务端的报文后，还要向服务端回应最后一个报文。首先该应答报文`TCP首部ACK标志位`置为1，其次确认应答字段填入`server_isn+1`，最后把报文发送给服务器，**这次报文可以携带客户端到服务器的数据，之后客户端处于`ESTABLISHED`状态；**

5. 服务端收到客户端的应答报文后，也进入`ESTABLISHED`状态

   ![TCP 三次握手](D:\Yang7hi\note\typora_photo\InterviewBook\TCP三次握手.drawio.png)

## TCP|为什么不能是两次握手

1. 三次握手才可以阻止重复历史连接的初始化
2. 同步双方初始序列号，保证可靠的同步
3. 避免资源浪费

**如何避免历史连接的：**

如果客户端发送SYN报文(例如seq=90)，然后客户端宕机了。而且整个SYN报文被网络阻塞了，服务端没有收到。接着客户端重启后，又重新向服务端建立连接，发送SYN(seq=100)报文。客户端连续的发送了多次建立连接的报文：

- 旧syn报文比新syn报文先抵达服务器，服务端会返回一个SYN+ACK的报文给客户端，这是确认报文为旧报文+1(91:90+1)
- 客户端收到后，发现自己的期望的确认号应该是(100+1)，而不是(90+1)，所以就会**回RST报文**
- 服务端收到RST报文后，就会释放连接
- 后续最新的SYN报文抵达服务端，通过100+1的确认报文完成第三次连接

如果RST报文来到服务器之前就来到了新的SYN报文，服务端会返回challenge ACK报文给客户端，仍然是上一个ack的确认号(90+1)。客户端收到后发送RST报文。

**二次握手为什么不可以**：

在俩次握⼿的情况下，服务端在收到SYN报⽂后就进⼊了ESTABLISHED状态，**意味着这时可以向对方发送数据**，**但是客户端此时并无法保证进⼊ESTABLISHED状态**，服务端在向客户端发送数据前，并没有阻止掉历史连接，导致服务端建⽴了⼀个历史连接，⼜白白发送了数据。  

理论上在不存在拥塞，重传SYN或历史连接问题是可以使用两次握手。但是显示网络通信中，两次握手并不安全，容易收到攻击。

## TCP|TCP四次挥手

1. 客户端打算关闭连接，此时会发送一个TCP首部FIN标志位置为1的报文，也即FIN报文，之后客户端进入`FIN_WAIT_1`状态；

2. 服务端收到FIN报文后，就向客户端发送ACK应答报文，接着服务端就进入CLOSE_WAIT状态

3. 客户端收到服务端的ACK应道报文之后，进入FIN_WAIT_2状态；

4. 等待服务端处理完数据后，也向客户端报送FIN报文，之后服务端就进入LAST_ACK状态；

5. 客户端收到服务端的FIN报文之后，回一个ACK报文，之后进入TIME_WAIT状态

6. 服务端收到ACK应答报文，进入CLOSE状态，至此服务端已经完成连接关闭；

7. 客户端经过2MSL时间后，自动进入CLOSE状态，至此客户端完成连接的关闭。

   ![客户端主动关闭连接 —— TCP 四次挥手](D:\Yang7hi\note\typora_photo\InterviewBook\format,png-20230309230614791.png)

## TCP|TCP四次挥手第二次和第三次不能合并吗

**在 TCP 协议的四次挥手过程中，第二次和第三次握手是不能合并的。**当被动关闭⽅在 TCP 挥⼿过程中，如果「没有数据要发送」，同时开启了 TCP 延迟确认机制「没有开启 TCP_QUICKACK（默认情况就是没有开启，没有开启 TCP_QUICKACK，<u>等于就是在使⽤ TCP 延迟确认机制</u>）」，那么第⼆和第三次挥⼿就会合并传输，这样就出现了三次挥⼿ 。**所以，出现三次挥⼿现象，是因为 TCP 延迟确认机制导致的。**  

[小林coding | 什么情况会出现三次挥手？](https://xiaolincoding.com/network/3_tcp/tcp_three_fin.html#%E5%AE%9E%E9%AA%8C%E9%AA%8C%E8%AF%81)

## TCP|为什么要三次握手 四次挥手

**三次握手**

- 阻止重复历史连接的初始化
- 同步双方的初始化序列
- 避免资源浪费

**四次挥手**：服务端通常需要等待完成数据的发送和处理，所以服务端ACK和FIN一般都会分开发送，因此需要四次挥手。

## TCP|网络编程函数

- socket():用于建立一个套接字，指定协议族和套接字类型

- bind():将套接字和本地地址（IP地址和端口号）绑定

- listen():将套接字设置为监听模式，等待连接请求

- accept():接受客户端的连接请求，创建一个新的套接字来处理与客户端的通信

- connect():向服务端发起连接请求

- send/write():向已连接的套接字发送数据

- ==recv/read()==:从已连接的套接字接受数据

- close():关闭套接字连接

- shutdown():优雅地关闭套接字连接，可选关闭读取/写入/读写。

## TCP|查看网络连接数

```shell
neststat -an | grep ESTABLISHED | wc -l
```

**`wc (word count)`**命令用于统计文件字节、字符、单词与行的数量

**`-l，--lines`** 仅显示行数

## TCP|UDP|什么时候用TCP UDP

- TCP面向连接。能保证数据可靠交付
  - FTP文件传输
  - HTTP/HTTPS
- UDP是无连接的，可以随时发送数据，且本身简单且高效
  - 包总量小的通信 DNS解析
  - 音视频多媒体通信（视频面试，如果丢包可以使用RUDP，结合TCP和UDP特性）
  - 广播通信

## UDP|丢包会有什么现象

- 传输的数据**无法到达**接收端，导致接收端获取**不到完整的数据包**
- 传输的数据包可能出现**乱序**，导致接收端**无法正确重构**数据
- 传输的数据包可能**重复发送**，导致接收端收到重复的数据
- 传输的数据包可能**出现错误**，导致接收端**无法正确解码**数据
- 应用程序性能降低


## Socket|Socket简介

Socket主要用来实现跨网络与不同主机上的进程间通信。Socket是在**应用层和传输层之间的一个抽象层**，他把TCP/IP层复杂的操作`抽象为几个简单的结构`供应用层调用以实现进程在网络中的通信。

在Linux系统中，可以通过Socket函数来打开一个网络文件，**返回值是一个文件描述符**，然后就可以使用普通的文件操作函数来传输数据。相较于成熟的协议（例如ISAPI、CGI、Winlnet、Winsock），可以实现网络延迟测试，直接控制网络底层的数据传输，控制细粒度更高。

## Socket|Socket如何建立

⽹络编程中常见的通信⽅式，但它也可以在同⼀台机器上的不同进程之间进⾏通信。

```cpp
int socket(int domain, int type, int protocol);
```

### 参数解释

1. **domain（协议族）**
   - **AF_INET**：用于 IPv4 网络协议。
   - **AF_INET6**：用于 IPv6 网络协议。
   - **AF_LOCAL / AF_UNIX**：用于本地通信（同一台机器上的进程间通信）。
2. **type（通信类型）**
   - **SOCK_STREAM**：字节流套接字，对应 TCP 协议。
   - **SOCK_DGRAM**：数据报套接字，对应 UDP 协议。
   - **SOCK_RAW**：原始套接字，允许程序直接访问底层协议，如 IP 协议。
3. **protocol（通信协议）**
   - 通常写为 0，因为协议已经通过前两个参数确定。

### TCP

1. 服务器和客户端初始化socket，得到文件描述符
2. 服务端调用bind，绑定IP地址和端口；调用listen，进行监听；调用accept，等待客户端连接
3. 客户端调用connect，向服务端的地址和端口发起连接请求
4. 服务端accept返回用于传输的socket文件描述符
5. 客户端调用write写入数据，服务端调用read读取数据
6. 客户端断开连接时，会调用close，那么服务端read读取数据时，会读到EOF，待处理完数据后，服务端调用close，表示连接关闭

### UDP

UDP没有三次握手，所以不需要调用listen和connect，但是UDP交互仍需要IP地址和端口号，因此需要bind

由于UDP来说，不需要维护连接，那么就没有所谓发送方和接受方，甚至都不存在客户端和服务端的概念。因此每一个UDP的socket都需要bind

## DNS 服务器用的是什么协议

大多数时候使用**UDP协议。**将主机域名转换为ip地址，属于**应用层协议**。

1.UDP 是无连接的，速度较快；2.大多数 DNS 查询和响应的数据量都很小，通常在 **512 字节**以内，这正好适合 UDP 的数据包大小限制。

扩展：DNS over TLS（DoT）/ DNS over HTTPS（DoH），增强隐私和安全性。

## ping命令用的是什么协议。在哪⼀层  

`ping` 命令用于测试主机之间的连通性和往返时间，使用的是 ICMP（Internet Control Message Protocol，互联网控制报文协议）。ICMP 属于网络层协议，即 OSI 模型的第三层。

1. 当你使用 `ping` 命令时，计算机会发送 ICMP 回显请求（Echo Request 类型为8）数据包到目标主机。
2. 目标主机接收到回显请求后，会回复一个 ICMP 回显应答（Echo Reply 类型为0）数据包。
3. 通过这些应答，`ping` 可以测量==到达目标主机所需的时间（往返时间）和数据包丢失率==，从而帮助诊断网络连接的质量和状态。

[小林coding | ping 的工作原理](https://xiaolincoding.com/network/4_ip/ping.html)

---



# 操作系统

## 存储系统 | 内存分页 缺页中断

缺页终端就是当软件试图访问一个已经映射在虚拟地址空间中，但并未加载到物理内存中的分页时，由中央处理器的内存管理单元所发出的中断。

- **软性页缺失**：
- **硬性页缺失：**
- **无效页缺失：**

## 什么是中断和异常  

中断和异常都会导致处理器暂停当前正在执行的任务，并转向执行一个特定的处理程序（中断处理程序&异常处理程序），然后在处理完这些特殊情况后，处理器会返回到被打断的任务继续执行。

### 中断**：**由计算机系统的外部事件触发的，通常和硬件设备相关。

- 目的：及时响应重要事件而暂时中断正常的程序执行

- 典型中断：时钟中断、IO设备中断（鼠标键盘）、硬件错误中断。操作系统通常会为每个类型中断分配一个中断处理程序。

- 中断会打断其他进程运行，并且中断处理程序响应中断时，可能还会**临时关闭中断**。也就是说，当前中断程序未执行完，系统中其他中断无法被响应，所以中**断可能发生丢失**。**要求中断程序短且快。**

- Linux解决：**中断处理程序执行过长和中断丢失。将中断分为上下半部**
  - 上半部：直接处理硬件请求，即硬中断。一般会关闭中断请求，主要负责耗时短工作，快速执行；
  - 下半部：内核触发，即软中断。负责上班未完成工作，以内核线程方式运行，延迟执行。


### 异常：由计算机系统的内部事件触发的，通常和正在执行的程序或指令有关

- 程序的非法操作、地址越位、运算符溢出等错误引起的事件
- 异常不能被屏蔽。出现异常，计算机系统暂停正常执行，转到异常处理程序处理异常。

## 进程和线程|进程和线程的区别  

- **调度**：线程是程序执⾏的基本单位；进程是拥有资源的基本单位。

- **并发性**：进程内部的多个线程并发； 不同进程之间切换实现并发，各⾃占有CPU实现并⾏

- **拥有资源**：线程不拥有系统资源，但单进程的多个线程可以共享属于进程的资源；进程是拥有资源的独⽴单位。

- **系统开销**：线程切换时只需保存和设置少量寄存器内容、程序计数器和栈指针，因此开销很小；进程需要切换虚拟地址空间，切换内核栈和硬件上下文等，开销很大

## 进程和线程|进程间通信

（进程间通信 IPC，Inter-Process Communication）

- **管道：**管道是一种半双工的通信方式，数据只能单向流动
  - 无名管道（内存文件）：只能在具有亲缘关系的进程之间使用，通常为父子进程
  - 有名管道（FIFO文件）：可以在不相关的进程之间交换数据
- **共享内存：**共享内存就是映射一段能被其他进程访问的内存，该段内存由一个进程创建，但是多个进程可以访问。共享内存是**最快的的IPC方式**，往往和信号量配合使用来实现进程间的同步和通信
- **消息队列：**是有消息的链表，存放在内核中并由消息队列识别符标识。消息队列克服了信号传递信息少，管道只能承载无格式字节流以及缓存区大小受限等缺点。
- **信号：**用于通知进程某个事件已经发生：比如`ctrl+c`就是一个信号量
- **信号量：**是一个计数器，用来控制多个进程对共享资源的访问
- **套接字：**适用于不同机器之间进程通信，在本地也可以作为两个进程的通信方式

|      通信机制      |                        描述                        | 优点                                         | 缺点                                                       |
| :----------------: | :------------------------------------------------: | :------------------------------------------- | :--------------------------------------------------------- |
|  无名管道 (Pipe)   | 单向通信，数据通过管道从一个进程传递到另一个进程。 | 简单易用，适用于父子进程间的通信。           | 只能用于有亲缘关系的进程间通信，数据传输速度较慢。         |
|  有命管道 (FIFO)   |      类似于管道，但可以用于任意进程间的通信。      | 支持无亲缘关系的进程间通信，使用简单。       | 需要创建和管理命名管道文件，数据传输速度较慢。             |
|      共享内存      |       多个进程共享一块内存区，实现高速通信。       | 通信速度快，数据存储在共享内存中，效率高。   | 需要同步机制来防止数据竞争，管理复杂。                     |
|      消息队列      |  进程通过发送和接收消息来通信，消息存储在内核中。  | 支持随机读取消息，有优先级机制，灵活性高。   | 消息长度受限，内核维护消息队列，占用系统资源。             |
|   信号 (Signal)    |            用于通知进程某个事件的发生。            | 简单高效，适用于异步事件通知。               | 信号量有限，信息量少，适用于简单通知，不适合大数据量传输。 |
| 信号量 (Semaphore) |     主要用于进程间的同步，可以实现互斥和同步。     | 提供了严格的同步机制，控制进程间的访问顺序。 | 仅用于同步，不能传递复杂信息，管理和使用较为复杂。         |
|  套接字 (Socket)   |        支持网络通信，可以在不同机器间通信。        | 功能强大，支持跨网络通信，适用于分布式系统。 | 通信开销较大，设置和使用较为复杂。                         |

## 进程和线程|线程间同步

线程同步机制是指在多线程编程中，为了防止线程之间可能会产出的竞态条件，保证互不⼲扰，而采用的⼀种机制。

常见的线程同步机制有以下几种：  

1. **互斥锁（Mutex）**：互斥锁是一种基本的同步机制，用于防止多个线程同时访问某个共享资源。当一个线程获取了互斥锁，其他线程必须等待直到该锁被释放。
2. **读写锁（Read-Write Lock）**：读写锁允许多个线程同时读取数据，但写入数据时需要独占访问。这可以提高性能，因为读操作通常比写操作频繁。
3. **信号量（Semaphore）**：信号量是一种计数器，用来表示可用资源的数量。**线程在访问资源前需要先获取信号量，访问完成后释放信号量。**对应的变量是一个整型（sem）变量。两个原子操作的系统调用函数来控制信号量的，分别是：
   - P 操作：将 sem 减 1，相减后，如果 sem < 0，则进程/线程进入阻塞等待，否则继续，表明 P 操作可能会阻塞；
   - V 操作：将 sem 加 1，相加后，如果 sem <= 0，则表明当前有阻塞中的进程，于是会将该进程唤醒运行；如果sem > 0，则表明当前没有阻塞中的进程；
4. **条件变量（Condition Variable）**：条件变量用于线程间的协调，允许一个或多个线程在某些条件成立之前挂起执行，并在条件满足时被唤醒。

## 进程和线程|条件变量怎么用

条件变量和锁是紧密相关的。

锁的使用场景：一个事同时只有一个人做，我抢到锁就我先进去操作，操作完毕再给下一个做

条件变量场景：

1. 首先，这件事情还是只能由一个人做，所以还是要用到**锁**，但线程抢到锁之后，发现还要**等待一些条件满足**才能做

2. 这时如果抢到的锁的线程一直循环检查这个条件，消耗高，而且每次检查完为了**不让CPU一直空跑**，需要sleep一下。

   sleep少，消耗高，sleep长，则有延迟，无法及时发现条件满足。抢到锁后的一直检查别的线程就无法拿到锁了

3. **引入条件变量**，抢到所得线程发现条件未满足时，释放锁，使用`pthread_cond_wait()`**挂起**自己。

   这时别的线程可以获取锁进来，发现条件不满足同样挂起；

   直到条件满足，由其他地方调用`pthread_cond_broadcast()`**来唤醒全部挂掉的线程**，或者调用`pthread_cond_signal`唤醒指定线程。

## 进程和线程|消费者生产者模型

一种多线程编程中的经典同步问题。其主要目的是在共享资源（通常是缓冲区）上协调多个生产者线程和消费者线程的操作。

- **生产者（Producer）**：生产者线程的任务是生成数据并将其放入共享缓冲区。当缓冲区满时，生产者线程需要等待直到缓冲区有空间。

- **消费者（Consumer）**：消费者线程的任务是从共享缓冲区中取出数据进行处理。当缓冲区为空时，消费者线程需要等待直到缓冲区中有数据。

```cpp
#include <pthread.h>
#include <iostream>
#include <queue>
#include <unistd.h>

using namespace std;

// 定义一个模板类，用于实现阻塞队列
template<typename T>
class BlockQueue {
private:
    // 检查队列是否已满
    bool isFull() {
        return que.size() == _capacity;
    }

    // 检查队列是否为空
    bool isEmpty() {
        return que.empty();
    }

public:
    // 构造函数，初始化队列容量及互斥锁和条件变量
    BlockQueue(int cap = 5) :_capacity(cap) {
        pthread_mutex_init(&_mutex, NULL);  // 初始化互斥锁
        pthread_cond_init(&_full, NULL);    // 初始化条件变量，表示队列满
        pthread_cond_init(&_empty, NULL);   // 初始化条件变量，表示队列空
    }
    
    // 析构函数，销毁互斥锁和条件变量
    ~BlockQueue() {
        pthread_mutex_destroy(&_mutex);     // 销毁互斥锁
        pthread_cond_destroy(&_full);       // 销毁条件变量 _full
        pthread_cond_destroy(&_empty);      // 销毁条件变量 _empty
    }

public:
    // 生产者调用此方法向队列中添加元素
    void push(const T& data) {
        pthread_mutex_lock(&_mutex);        // 加锁，进入临界区
        while (isFull()) {                  // 如果队列已满，则生产者等待
            pthread_cond_wait(&_full, &_mutex);
        }
        que.push(data);                     // 添加元素到队列
        pthread_cond_signal(&_empty);       // 通知消费者队列非空
        pthread_mutex_unlock(&_mutex);      // 解锁，离开临界区
    }
    
    // 消费者调用此方法从队列中取出元素
    void pop(T& data) {
        pthread_mutex_lock(&_mutex);        // 加锁，进入临界区
        while (isEmpty()) {                 // 如果队列为空，则消费者等待
            pthread_cond_wait(&_empty, &_mutex);
        }
        data = que.front();                 // 取出队列头部的元素
        que.pop();                          // 弹出队列头部的元素
        pthread_cond_signal(&_full);        // 通知生产者队列非满
        pthread_mutex_unlock(&_mutex);      // 解锁，离开临界区
    }

private:
    queue<T> que;                           // 队列容器
    int _capacity;                          // 队列的容量
    pthread_mutex_t _mutex;                 // 互斥锁，用于保护临界区
    pthread_cond_t _full;                   // 条件变量，表示队列满
    pthread_cond_t _empty;                  // 条件变量，表示队列空
};

```



##  Linux中为什么设计了内核态和用户态  

处于安全性和稳定性考虑。

- **安全性** 内核态具有最高的特权级别。1.能够执行所有的CPU指令并访问所有的内存地址；2.内核态可以直接访问硬件设备；3.某些特权指令只有在内核态才能执行，例如内存管理（如设置页表）、设备控制（如I/O端口访问）、中断处理和系统状态设置（如设置时钟）。

  而**用户态**具有较低的特权级别，限制了对系统资源的直接访问。**`这虽然增加了一些开销，防止用户程序进行不安全或非法的操作，导致系统崩溃`**

- **稳定性** 如果用户态的程序出现错误或崩溃，**`只会影响该特定程序`**，而不会影响到内核或其他用户态程序。操作系统可以在用户程序出错时采取措施进行**`恢复`**，例如通过进程隔离和内存保护机制来捕获和处理异常.

- **资源管理**  内核态**`负责管理系统的硬件资源`**，如CPU、内存、硬盘和网络设备等，负责进程调度和上下文切换。用户态程序必须通过系统调用（system call）请求内核服务。

## 有什么方式进行内核态和用户态的切换

- 系统调用（System Call）
  由于某些操作（如文件操作、进程控制、内存管理等）需要访问受保护的系统资源，而用户态无法直接完成这些操作，因此需要通过系统调用切换到内核态执行。**主动发起的请求**
- 出现异常
  用户程序在执行过程中发生的**`意外情况`**（如除零错误、非法内存访问等），需要内核处理。发生异常时，CPU会自动切换到内核态，执行内核中的异常处理程序。**系统被动地进行处理**
- 外围设备中断
  外围设备（如硬盘、网络接口、键盘等）发出中断信号时，当前**`正在执行的用户态进程会被暂停`**，CPU切换到内核态处理中断。中断处理完成后，CPU可能会**`继续执行`**被暂停的用户态进程。

## epoll是同步的还是异步的
1. **从I/O层⾯来看，EPOLL⼀定是同步的**。通过 `epoll_wait` 函数来等待事件发生。当调用 `epoll_wait` 时，如果没有任何事件发生，**调用进程会被阻塞，直到有事件被触发或超时。**从这个角度来看，epoll 本身并不改变 I/O 操作的同步特性，即 I/O 操作需要等待直到完成。
2. **从消息处理层面来看，EPOLL是异步的**。允许应用程序注册感兴趣的 I/O 事件，并在发生时才进行处理，而不是持续轮询。是一种事件驱动的异步处理。内核会跟踪所有注册的文件描述符。

## 小根堆（小顶堆）定时器，如果⼀次Pop⼀个的话，高并发情况下会不会有问题  

可能会遇到一些问题，特别是当多个线程同时访问和操作堆时。

**锁竞争** 多个线程同时对小根堆进行 `pop` 操作而不加锁，可能会导致竞态条件。大量线程竞争同一个锁，会导致锁争用和上下文切换频繁，影响整体性能。

**解决方案** 

1. 考虑细粒度锁，而不是整个堆加锁；

2.  使用时间轮定时器：划分为多个槽（bucket），每个槽代表一个时间间隔，将定时任务放入相应的槽中

## 互斥和同步|`死锁`产生的条件是什么？C++怎么避免死锁

**死锁：**两个或多个并发的进程，如果每个进程持有某个资源的同时又在等待其他的进程释放资源，导致程序无法向前推进，称这组进程产生了死锁。简单地说，两个或者多个进程无限期地阻塞，互相等待的状态。

### **四个条件，同时满足**

- **互斥：**一个资源只被一个进程使用
- **不可抢占：**进程已获资源，在未使用完成钱不能被强行剥夺
- **占有并等待：**一个进程因请求资源而阻塞，对已获得资源保持不放
- **循环等待：**若干进程之间形成一种首尾相接的循环等待资源关系

### **避免死锁**

- 破坏互斥条件：修改代码使得资源可以被多个资源并行使用，**例如使用共享内存**
- 破坏不可抢占：在一些情况下，**允许系统强制剥夺进程所占有的资源**
- 破坏占用并等待：进程请求资源时候，**先释放已经请求的资源，再去请求新的资源**
- 破坏循环等待：按照一定的规则对资源进行排序，按照进程按照相同顺序请求资源。

## 互斥和同步|介绍⼀下你知道的锁

**两个基础的锁，三个特殊锁：**  

1. **互斥锁：**互斥锁是一种常见的锁类型，用于实现互斥访问共享资源。在任何时候只有一个线程可以持有互斥锁，其他线程必须等待直到锁释放。确保了同一时间只有一个线程能够访问被保护的资源；

​	**互斥锁加锁失败的线程会「线程切换」**。从用户态陷入到内核态，让内核帮我们切换线程，有**两次线程上下文切换的成本**。

`如果你能确定被锁住的代码执行时间很短，就不应该用互斥锁，而应该选用自旋锁，否则使用互斥锁。`

2. **自旋锁：**通过 CPU 提供的 `CAS` 函数（*Compare And Swap*），在「用户态」完成加锁和解锁操作，**不会主动产生线程上下文切换**，所以相比互斥锁来说，会快一些，开销也小一些。**加锁失败的线程会「忙等待」**；

3. **读写锁：**允许多个线程**共同读资源**，只允许**一个线程写操作**。分为读(共享)和写(排他)两种状态；

4. **悲观锁：**认为多线程同时修改共享资源的概率比较高，于是很容易出现冲突。工作方式：访问共享资源前，先要上锁。

5. **乐观锁：**认为多线程同时修改共享资源的概率比较低，于是不容有出现冲突。工作方式：先修改完共享资源，再验证这间内有没有发生冲突，如果没有其他线程在修改资源，那么操作完成，如果发现有其他线程已经修改过这个资源，就放弃本次操作。**乐观锁全程并没有加锁，所以它也叫无锁编程**。

   **如何实现乐观锁**：

   - **版本号（Version Number）机制:**为数据库表增加⼀个数字类型的 “version” 字段来实现。当读取数据时，将version字段的值⼀同读出，数据每更新⼀次，对此version值加⼀。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第⼀次取出来的version值进行比对，如果数据库表当前版本号与第⼀次取出来的version值相等，则予以更新，否则认为是过期数据。
   - 常见的 SVN 和 Git 也是用了乐观锁的思想，先让用户编辑代码，然后提交的时候，通过版本号来判断是否产生了冲突，发生了冲突的地方，需要我们自己修改后，再重新提交。

[REF：小林coding|互斥锁与自旋锁](https://xiaolincoding.com/os/4_process/pessim_and_optimi_lock.html#%E4%BA%92%E6%96%A5%E9%94%81%E4%B8%8E%E8%87%AA%E6%97%8B%E9%94%81)

## 讲一下协程

协程是用户态的轻量级线程，是线程内部调度的基本单位。

同一个时间只能执行一个协程，而其他协程处于休眠状态，适合对任务进行分时处理。

- ==协程是由用户代码控制的，而不是由操作系统内核调度==。它不依赖操作系统提供的线程管理机制，因此可以在用户空间实现，并避免线程切换的**开销**；

- 可以在==一个线程内执行多个协程==，并且可以通过协程之间的切换来实现任务并发。这种切换是协作式的，也就是一个协程在执行过程中可以==**主动让出执行权**==给其他协程。

  **协程作用**

  1. 异步编程：简化异步任务的调度和等待，使代码看起来像是顺序执行的。传统的异步编程通常涉及回调函数，这可能会导致回调地狱（Callback Hell）；
  2. 状态机：协程能够暂停和恢复执行，保留函数的上下文状态，从而能够编写具有复杂状态转换逻辑的代码，而无序显式地编写状态机的逻辑；
  3. 生成器：协程可以作为生成器（Generator）使用，通过逐步生成数据而不是一次性生成所有数据。对于处理大量数据或按需计算的数据非常有用
  4. 轻量级线程：协程是一种轻量级的并发机制，可以用于`并发任务的调度和执行`。与线程相比，协程的创建和`切换开销较小`，且协程通常在同一线程内运行，避免了线程间的上下文切换开销。也`减少`了由于共享状态引起的`线程安全`问题。

## 什么是QPS和TPS，如何计算

- QPS是每秒查询率，值一台服务器每秒能响应的查询次数，用于衡量特定的查询服务器在规定事件内处理流量的多少，是主要针对专门用于查询服务器的性能指标；
  - QPS = 1s/单个请求耗时 * 服务器核心数（线程数）
- TPS是每秒事务数，一个事务市值客户端向服务器发送请求然后服务器做出反应的过程
  - TPS = 事务的数量 / 执行的事件

---



# MYSQL

## 默认端口

MySQL 端口 6000

## MYSQL索引

索引是帮助存储系统引擎快速获取数据的一种数据结构，索引是数据的⽬录。

从 MySQL 5.5 开始，**InnoDB** 成为默认的存储引擎。

**InnoDB**主要使用B+Tree索引类型，创建表时，**InnoDB**存储引擎会根据不同的场景选择不同的列作为索引。

- 若果有主键，默认使用**主键**作为聚簇索引的索引键
- 若没有主键，就选择一个==**`不含有NULL值的唯一列`**==作为聚簇索引的索引键
- 若两个都没有，**InnoDB**将会自动生成一个**隐式自增ID列**作为聚簇索引的索引键

其他索引称为辅助索引，或二级索引，非聚簇索引

B+ Tree是一个多叉树，叶子节点才存放数据，非叶子节点只存放索引，且每个节点里的数据都是按主键顺序存放的。每一层父节点的索引值都会出现在下层子节点的索引值中，所以**叶子节点包含所有的索引值信息**，并且**每一个叶子节点都有两个指针**，分别指向下一个叶子节点和上一个叶子节点，形成一个双向链表，从而有利于进行范围搜索。

![img](D:\Yang7hi\note\typora_photo\InterviewBook\997909-20190728114240297-169990922.png)

在二级索引的B+Tree就能查询到结果的过程叫做**覆盖索引**，也就是只需要查一个B+Tree就能找到数据联合索引，存在最左匹配原则（在联合索引中，查询条件必须从索引的最左边列开始匹配，并且按索引中列的顺序连续匹配）。

## MySQL的buffer pool怎么清除

```mysql
FLUSH BUFFER POOL
```

清空所有的缓存页，使得MySQL需要重新从磁盘读取磁盘中的数据，下一次查询速度自然会变慢。

## MySQL的事务是什么

在数据库中。事务是一组操作单元，这些操作单元要么全部执行完成，要么全部执行失败。事务是保证数据库一致性的重要机制之一，它可以将一系列的操作看作一个整体，从而保证数据库的完整性和正确性。

- 原子性

- 一致性

- 隔离性

- 持久性

---

---



# Redis

## 什么是Redis, 具有哪些特点

Redis是一个基于内存的数据库，读写速度非常快，通常被用作缓存、消息队列、分布式锁和键值存储数据库。

它支持多种数据结构，如字符串、哈希表、列表、集合、有序列表等

Redis还提供了分布式特性，可以将数据分布在多个节点上，以提高可扩展性和可用性。

### 在应用中的使用

1. 用作缓存层，存储频繁访问的数据，提高访问速度；
2. 用作消息队列，通过发布订阅模式传递消息；
3. 存储会话数据、计数器等

## Redis 与 Memcached 有什么区别

**共同点**

1. 都是基于内存的数据库，一般都用来做缓存使用
2. 都有过期策略
3. 两者的性能都非常高

**区别**

- Redis 支持的数据类型更为丰富，Memcached只支持简单key-value数据类型；
- Redis 支持数据的持久化，可以将内存数据保持在磁盘中，重启再次载入可用；Memcached则没有；
- Redis 支持原生集群模式，Memcached则没有，需要依靠客户端来实现集群中的分片写入数据
- Redis 支持发布订阅模型、Lua脚本、事务等功能，Memcached不支持

- 

## 数据结构|使用场景

![img](D:\Yang7hi\note\typora_photo\InterviewBook\key.png)

| 结构类型     | 结构存储的值                               | 结构读写能力                                                 | 应用场景                                                     |
| ------------ | ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| String字符串 | 可以是字符串、整数或浮点数                 | 对整个字符串或字符串中的一部分进行操作；对整数或浮点数进行自增或自减操作 | 缓存对象、常规计数、分布式锁、共享session信息                |
| List列表     | 一个链表，链表上的每个节点都包含一个字符串 | 对链表两端进行push和pop操作，读取单个或多个元素；根据值查找和删除元素 | 消息队列（两个问题：1. 生产者需要自行实现全局唯⼀ID； 2.不能以消费组形式消费数据） |
| Set集合      | 包含字符串的无序集合                       | 字符串的集合，方法：查找、是否存在、添加、删除；还包括**聚合计算** | 聚合计算（并集、交集、差集），例如点赞、共同关注、抽奖活动   |
| Hash散列     | 包含键值对的无序散列表                     | 添加、获取、删除单个元素                                     | 缓存对象、购物车                                             |
| Zset有序集合 | 和散列一样，用于存储键值对                 | 字符串成员与浮点数分数之间的有序映射；元素的排列顺序由分数大小决定；方法：添加、获取、删除单个元素以及 **范围或成员获取元素** | 排序场景，例如排行榜                                         |

Redis版本更新，又增加了几种数据类型：

- BitMap：存储位的数据结构，可以用于处理⼀些位运算操作；例如签到、判断用户登陆状态
- HyperLogLog：海量数据基数统计；例如百万级网页 UV 计数等
- GEO：存储地理位置信息的数据结构
- Stream：专门为消息队列设计的数据类型，解决了List列表两个问题

## 数据结构|类型内部实现

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\9fa26a74965efbf0f56b707a03bb9b7f.png" alt="img" style="zoom:50%;" />

### String 

String 类型的底层的数据结构实现主要是 SDS（简单动态字符串）实现

**SDS 相比于 C 的原生字符串，增加了三个元数据：len、alloc、flags**

- SDS 的 buf[] 字符数组，不仅可以保存文本数据，还可以保存二进制数据。
- SDS 使用 len 属性的值而不是空字符来判断字符串是否结束，因此它也保存这长度信息
- SDS 获取字符串长度的时间复杂度是 O(1)，c是O(n)
- SDS 拼接字符串不会造成缓冲区溢出，支持动态扩展。`alloc - len` 计算剩余空间

**<img src="D:\Yang7hi\note\typora_photo\InterviewBook\image-20240725232549832.png" alt="image-20240725232549832" style="zoom:50%;" />**

### List

List 类型的底层数据结构是由**~~双向链表或压缩列表~~**->==quicklist==

- 如果列表的**元素个数**小于 512 个（默认值，可由 list-max-ziplist-entries 配置），**所有值**小于 64 字节（默认值，可由 list-max-ziplist-value 配置），使用**压缩列表**作为 List 类型的底层数据结构；
- 如果列表的元素不满足上面的条件，使用**双向链表**作为 List 类型的底层数据结构；
- **Redis 3.2 版本之后，List 数据类型底层数据结构就==只由== quicklist 实现了**

### Hash

Hash 类型的底层数据结构是由**~~压缩列表~~或哈希表**->==listpack==

- 如果哈希类型**元素个数**小于 512 个（默认值，可由 hash-max-ziplist-entries 配置），**所有值**小于 64 字节（默认值，可由 hash-max-ziplist-value 配置）的话，使用**压缩列表**作为 Hash 类型的底层数据结构；
- 如果哈希类型元素不满足上面条件，使用**哈希表**作为 Hash 类型的底层数据结构。
- **Redis 7.0 中，压缩列表交由 listpack 数据结构来实现了**。

### Set

Set 类型的底层数据结构是由**哈希表或整数集合**实现

- 如果集合中的元素**都是整数且元素个数**小于 512 （默认值，set-maxintset-entries配置）个，使用**整数集合**作为 Set 类型的底层数据结构；
- 如果集合中的元素不满足上面条件，则 Redis 使用**哈希表**作为 Set 类型的底层数据结构。

### ZSet

Zset 类型的底层数据结构是由**~~压缩列表~~或跳表**->**==listpack==** 

- 如果有序集合的**元素个数小于 128 个**，**所有值**小于 64 字节时，使用**压缩列表**作为 Zset 类型的底层数据结构
- 如果有序集合的元素不满足上面的条件，使用**跳表**作为 Zset 类型的底层数据结构
- **Redis 7.0 中，压缩列表交由 listpack 数据结构来实现了**

## 数据结构|压缩列表 跳表 quicklist listpack

### 压缩列表

**由连续内存块组成的顺序型数据结构**，有点类似于数组

在压缩列表中，如果我们要查找定位第一个元素和最后一个元素，可以通过表头三个字段（zllen）的长度直接定位，复杂度是 O(1)。而**查找其他元素时，就没有这么高效了，只能逐个查找，此时的复杂度就是 O(N) 了，因此压缩列表不适合保存过多的元素**。

**缺点：**

- 如果保存的元素数量增加了，或是元素变⼤了，会导致**内存重新分配**，会有**==连锁更新==**的问题，**直接影响到压缩列表的访问性能**。  
- **压缩列表只会用于保存的节点数量不多的场景**

![img](D:\Yang7hi\note\typora_photo\InterviewBook\1720432496274-b95e1802-1ecd-4210-a987-733265534c64.png)

### 跳表

跳表是⼀种在链表基础上改进过来的，实现了⼀种==「多层」的有序链表==，当数据量很⼤时，跳表的查找复杂度就是O(logN)。用于实现有序集合（Sorted Set）。

![img](D:\Yang7hi\note\typora_photo\InterviewBook\1719804939236-89f12a47-b851-4d06-a5f3-399e1119db57.png)

### 为什么只用跳表而不是红黑树

- 内存占用：插入和删除操作上的性能表现好，**跳表比平衡树更灵活一些**。
- 范围查找：跳表天然⽀持按范围查找，**跳表比平衡树操作要简单**
- 算法实现：**跳表比平衡树简单多**

### listpack

没有压缩列表中记录前⼀个节点长度的字段， listpack 只记录当前节点的长度。

当我们向 listpack 加入⼀个新元素的时候，不会影响其他节点的长度字段的变化，避免了**压缩列表的连锁更新**问题。  

### quicklist

**双向链表 + 压缩列表组合，** quicklist 就是⼀个链表，而链表中的每个元素又是⼀个压缩列表。 

quicklist 解决办法：通过控制每个链表节点中的压缩列表的大小或者元素个数，来规避连锁更新的问题。因为压缩列表元素越少或越小，连锁更新带来的影响就越小，从而提供了更好的访问性能  

![img](D:\Yang7hi\note\typora_photo\InterviewBook\1719035634188-584809ba-ea0b-48ff-a547-9ee4d1b4d365.png)

## 线程模型|Redis 采用单线程那么快

- Redis 的大部分操作都在内存中完成，并且采用了高效的数据结构，因此Redis瓶颈可能是机器的内存和网络宽带，而非CPU。CPU不再是瓶颈，这也是为什么采用单线程解决方案的原因；
- Redis 采用单线程的模型可以避免多线程之间的竞争，省去了多线程切换在时间和性能开销，同时不会产生死锁；
- Redis 采用IO多路复用机制处理大量客户端的Socket请求。

## Redis持久化|持久化机制

- **AOF日志**：每执行一条写操作，就把该命令以追加的方式写入到一个文件里；
- **RDB快照：**将某一时刻的内存数据，以二进制的方式写入磁盘；
- **混合持久化：**Redis 4.0新增的方式，集成了AOF和RDB的优点

AOF 日志**减低了大量数据丢失的风险**，但是因为记录的是操作命令，不是实际的数据，所以用 AOF 方法做故障恢复时，需要全量把日志都执行一遍，一旦 AOF 日志非常多，势必会造成 Redis 的恢复操作缓慢。

**RDB 优点是数据恢复速度快**，但是快照的**频率不好把握**。频率太低，丢失的数据就会比较多，频率太高，就会影响性能。

混合持久化，AOF 文件的**前半部分是 RDB 格式的全量数据，后半部分是 AOF 格式的增量数据**。优点：以上两个；缺点：兼容性差

## Redis持久化|AOF写回策略

| 写回策略 | 写回时机           | 优点                             | 缺点                               |
| -------- | ------------------ | -------------------------------- | ---------------------------------- |
| Always   | 同步写回           | 可靠性高，最大程度保证数据不丢失 | 每个写命令都要写回硬盘，性能开销大 |
| Everysec | 每秒写回           | 性能适中                         | 宕机时会丢失1s内的数据             |
| No       | 由操作系统控制写回 | 性能好                           | 宕机时丢失的数据可能会很多         |

- Redis 为了避免 AOF 文件越写越大，提供了 **AOF 重写机制**
- 使用重写机制后，就会读取**最新的 key-value（键值对）**，然后用一条命令记录到新的 AOF 文件
- 重写 AOF 过程是由后台子进程 *==bgrewriteaof==* 来完成的

## Redis集群|Redis 如何实现服务高可用

### 主从复制

- 将一个Redis实例的数据复制到其他实例，其中一个是主节点，其余是从节点。

- **主节点将==写操作传播==到所有从节点：**主服务器可以进行读写操作，当发生写操作时自动将写操作同步给从服务器，而从服务器一般是只读，并接受主服务器同步过来写操作命令，然后执行这条命令。

- **主从数据不会时时刻刻保持一致**：主服务器并不会等到从服务器实际执行完命令后，再把结果返回，而是本地执行完命令后，就会向客户端返回。这导致无法实现强一致性保证，==**数据不一致**==是难以避免的。

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\2b7231b6aabb9a9a2e2390ab3a280b2d.png" alt="img" style="zoom:33%;" />

### 哨兵模式

- 监控 Redis 实例的状态，发现主节点的故障并自动进行故障转移 。
- **监控：**监控 Redis 主服务器和从服务器状态，包括连接状态、是否能执行命令、是否持久化；
- **故障转移**：当 Sentinel (哨兵)发现主服务器宕机，他会通过一定的选举机制选择一个从服务器升级为新的主服务器，然后通知其他从服务器切换到新主服务器。

### 切片集群(Redis Cluster)

- 将数据分布在不同的服务器上，以此来降低系统对单主节点的依赖

采用哈希槽（Hash Slot），来处理数据和节点之间的映射关系,**一个切片集群共有 16384 个哈希槽**;

- 根据键值对的 key，按照 CRC16 算法得到16bit值
- 再用 16bit 值对 16384 取模，得到 0~16383 范围内的模数，每个模数代表一个相应编号的哈希槽

哈希槽怎么被映射到具体的 Redis 节点

- 平均分配：9个节点，每个节点槽为；手动分配：需要把 16384 个槽都分配完，否则 Redis 集群无法正常工作

- **优点：**
  1. 无中心架构：Redis Cluster采用P2P模式，完全去中心化，所有的Redis节点彼此互联，内部使用二进制协议优化传输速度和带宽
  1. 数据分片：数据按照slot存储分布在多个节点，节点间数据共享，可动态调整数据分布
  1. 可扩展性：可线性扩展到1000多个节点，节点可动态添加或删除
  1. **高可用性**：部分节点不可用时，集群仍可用

- **缺点：**集群同步问题、部署和维护较复杂

## Redis集群|哨兵工作原理

1. 判断节点是否存活

   每个Sentinel定期向Redis集群所有节点发送PING命令。如果哨兵连续一定次数未收到服务器的响应，则认为**==故障节点主观下线==；**

   被一个Sentinel节点记为主观下线时，并不意味着该节点肯定故障了，还需要Sentinel**集群**的其他Sentinel节**点共同判断**为主观下线才行；

   Sentinel集群中超过quorum数量的Sentinel节点认为该redis节点主观下线，则该redis**==故障节点客观下线==。**

   <img src="D:\Yang7hi\note\typora_photo\InterviewBook\1720159526929-d21d0b01-4ed9-4af7-8278-9d7fbdc4db3b.png" alt="img" style="zoom:50%;" />

2. 选取新主节点Leader

   当发现主服务器下线后，哨兵会协调选举出一个新的主服务器。在这过程中，哨兵会考虑每个可用的从服务器，选择一个作为新的主服务器

   具体过程

   - **选择候选服务器：**哨兵会从可用的从服务器中选择⼀组==**候选服务器**==，通常选择**复制偏移量（replication offset）最大的从服务器**。  
   - **计算投票：**==**每个哨兵为每个候选从服务器投票**==。投票的考量因素包括从服务器的复制偏移量、连接质量、优先级等；
   - **达成共识**

3. 更新配置信息，通知客户端

   ⼀旦新的主服务器被选出，哨兵会更新 Redis 集群的配置信息，包括将 ==**新的主服务器的地址和端口**== 通知给 其他哨兵和客户端。  

   告知客户端新的主服务器的位置，以便客户端重新连接

## Redis缓存|缓存雪崩

**==大量缓存数据在同一时间过期（失效）==**时，如果此时有==**大量的用户请求，都无法在 Redis 中处理**==，于是全部请求都==**直接访问数据库**==，从而导致数据库的压力骤增，严重的会造成数据库宕机，从而形成一系列连锁反应，造成整个系统崩溃，这就是**缓存雪崩**

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\e2b8d2eb5536aa71664772457792ec40.png" alt="img" style="zoom:50%;" />

### 解决方案

1. 大量缓存数据同时过期
   - 均匀设置过期时间，将缓存失效时间随机打散
   - 互斥锁：如果发现访问数据不在Redis内，加互斥锁，保证同一时间只有一个请求构建缓存
   - 后台更新缓存：让缓存永久有效，更新缓存交由后台线程定时更新
2. Redis 故障宕机
   - 服务熔断或请求限流
     - 服务熔断：暂停业务对缓存服务访问，直接返回错误
     - 请求限流：只将少部分的请求发送到数据库进行处理，再多的请求就在入口直接拒绝服务
   - 构建Redis缓存高可靠集群
     - 通过主从节点构建缓存高可靠群
     - 若主节点故障宕机，从节点可以切换成为主节点，继续提供缓存服务

## Redis缓存|缓存击穿

**缓存击穿：**某个**热点数据**过期了，此时大量的请求访问了该热点数据，就无法从缓存中读取，直接访问数据库，数据库很容易就被高并发的请求冲垮。

缓存击穿是缓存雪崩的⼀个⼦集。**解决方案：**添加互斥锁或者后台更新缓存。

## Redis缓存|缓存穿透

**缓存穿透：**要访问的数据**既不在缓存中，也不在数据库中**，这样就无法构建缓存数据，后续所有的请求都会向数据库查找，增加数据库的负载。

**出现原因** 

- 业务误操作，缓存中的数据和数据库中的数据都被误删除了
- 恶意攻击，通过构造不存在key大量访问缓存，导致对数据的频繁查询

**解决方案**

- 对非法请求做限制
- 针对查询数据，在缓存中设置一个空值或者默认值
- 使用布隆过滤器快速判断数据是否存在，避免数据库判断数据是否存在

<table class="tg"><thead>
  <tr>
    <th class="tg-0pky">缓存异常</th>
    <th class="tg-0pky">产生原因</th>
    <th class="tg-0pky">应对方案</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-0pky" rowspan="2">缓存雪崩</td>
    <td class="tg-0pky">大量缓存数据同时过期</td>
    <td class="tg-0pky">- 均匀设置过期时间，将缓存失效时间随机打散<br>- 互斥锁：如果发现访问数据不在Redis内，加互斥锁，保证同一时间只有一个请求构建缓存<br>- 后台更新缓存：让缓存永久有效，更新缓存交由后台线程定时更新</td>
  </tr>
  <tr>
    <td class="tg-0pky">Redis 故障宕机</td>
    <td class="tg-0pky">- 服务熔断或请求限流<br>- 构建Redis缓存高可靠集群</td>
  </tr>
  <tr>
    <td class="tg-0pky">缓存击穿</td>
    <td class="tg-0pky">拼房访问的热点数据过期</td>
    <td class="tg-0pky">- 互斥锁：如果发现访问数据不在Redis内，加互斥锁，保证同一时间只有一个请求构建缓存<br>- 后台更新缓存：让缓存永久有效，更新缓存交由后台线程定时更新</td>
  </tr>
  <tr>
    <td class="tg-0pky">缓存穿透</td>
    <td class="tg-0pky">要访问的数据既不在缓存中，也不在数据库中</td>
    <td class="tg-0pky">- 对非法请求做限制<br>- 针对查询数据，在缓存中设置一个空值或者默认值<br>- 使用布隆过滤器快速判断数据是否存在，避免数据库判断数据是否存在</td>
  </tr>
</tbody>
</table>
## Redis缓存|缓存更新策略

常见的缓存更新策略共有3种：

- Cache Aside（旁路缓存）策略；
- Read/Write Through（读穿 / 写穿）策略；
- Write Back（写回）策略；

实际开发中，Redis 和 MySQL 的更新策略用的是 Cache Aside，另外两种策略应用不了。

### Cache Aside（旁路缓存）策略

Cache Aside（旁路缓存）策略是最常用的，应用程序直接与「数据库、缓存」交互，并负责对缓存的维护，该策略又可以细分为「读策略」和「写策略」。

## Redis缓存|保证数据库和缓存的一致性

==**读数据**==，我会选择Cache Aside（旁路缓存）策略。如果 cache 不命中，会从 db 加载数据到 cache。

==**写数据**==，我会选择更新 db 后，再删除缓存。

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\1720197145123-7e06f9a4-fcc7-42cc-a3d2-c44af4a73214.webp" alt="img" style="zoom: 67%;" />

如果需要数据库和缓存数据保持**强一致**，就不适合使用缓存。所以使用缓存提升性能，就是会有数据更新的延迟

**删除缓存异常**的情况，可以使用 2 个方案避免：

- 删除缓存重试策略（消息队列）
- 订阅 MySQL binlog，再删除缓存（Canal+消息队列）

---



# 设计模式

## 单例模式

为保证一个类只有一个实例，并提供一个访问它的全局访问点，该实例被所有程序模块共享。

### 懒汉版

==先不创建实例，在第一次被调用时，才创建实例。==直到调用`get_instance()`方法的时候才 new一个单例的对象

```cpp
class A{
private:
    A(){}   // 私有构造函数，无法直接创建对象
    static A* instance;
public:
    static A* Getinstance() {
        if (instance == NULL){
            instance = new A();
        }
        return instance;
    }
};

A* A::instance = NULL;

int main() {
    A* a1 = A::Getinstance();
    A* a2 = A::Getinstance();
    if (a1 == a2) std::cout << 'test' << std::endl;
}
```

**优点**：延迟实例化，不需要使用就不会被实例化，只有需要时才创建实例，避免资源浪费；

|       缺点       |                             描述                             |                           解决办法                           |
| :--------------: | :----------------------------------------------------------: | :--: |
|  **线程不安全**  | 多线程获取单例时候可能引发竞态条件。同时两个线程判断**`Getinstance`**都为空。 | 加锁 |
| **内存泄露问题** | 类只负责new出对象，之后没有调⽤delete释放，**因此只有构造函数被调用，析构函数没有被调用**，因此会造成内存泄漏。 | 1.使用共享指针`unique_ptr `(单例其实使用unique_ptr )<br />**2. 使用局部静态变量** |

```cpp
class A {
private:
    A() {}  // 私有构造函数，无法直接创建对象
    static std::shared_ptr<A> instance;
    static std::mutex mtx;  // 用于线程安全
public:
    static std::shared_ptr<A> GetInstance() {
        if (!instance) {
            std::lock_guard<std::mutex> lock(mtx); // 锁定
            if (!instance) {
                instance = std::shared_ptr<A>(new A());
            }
        }
        return instance;
    }
};

// 初始化静态成员
std::shared_ptr<A> A::instance = nullptr;
std::mutex A::mtx;

int main() {
    std::shared_ptr<A> a1 = A::GetInstance();
    std::shared_ptr<A> a2 = A::GetInstance();
    if (a1 == a2) std::cout << "test" << std::endl; // 输出 "test"
}
```

### 饿汉式

无论需不需要这个实例，直接先实例好，当需要使用时，直接调用方法即可。

**优点**：避免了线程不安全问题

**缺点**：资源浪费

```cpp
// 饿汉模式
class singleHunger{
private:
    int dataA;
    int dataB;

    // 构造函数私有化
    singleHunger(int a, int b):
        dataA(a),
        dataB(b){}

    // 禁用默认拷贝构造、赋值运算符符号
    singleHunger(const singleHunger& s) = delete;
    singleHunger& operator = (const singleHunger& s) = delete;
public:
    ~singleHunger(){}
    static singleHunger s_instance;
    static singleHunger& getInstance();

    int getAddResult(){
        int res = dataA + dataB;
        std::cout << "add res = " << res << std::endl;
        return res;
    }
};

// 定义静态成员变量
SingleHunger SingleHunger::s_instance(0, 0);

int main() {
    // 创建单例实例
    SingleHunger::createInstance(10, 20);
    // 获取并打印dataA和dataB的和
    std::cout << "Result from getInstance: " << SingleHunger::getInstance().getAddResult() << std::endl;
    return 0;
}
```

潜在问题在于static singleHunger s_instance;和 getAddResult 二者的初始化顺序不确定，如果在初始化完成之前调用getInstance方法会返回一个未定义的实例。

> [Ref| C++ 单例模式总结与剖析](https://www.cnblogs.com/ybqjymy/p/14921444.html)

## 工厂模式

常用的创建型涉及模式，提供了创建对象的最佳方式。创建对象并不会堆客户端暴露对象的创建逻辑，而是通过使用共同的接口来创建对象。

**分类：**简单工厂模式，工厂方法模式，抽象工厂模式  

## 设计模式原则

- 单一职责模式

---



# `Hopechart`

## 盲区识别 BSD 算法 | Blind Spot Detection

![在这里插入图片描述](D:\Yang7hi\note\typora_photo\InterviewBook\c25acd20c1ce49d69a65828634cb0d5d.png)

- **B线应平行于测试车辆的后缘，并在其后方3.0米处。**

- **C线应平行于测试车辆的前缘，并位于第95百分位眼睑的中心。**

- **F线应平行于测试车辆的中心线，并与测试车辆车身最左外边缘左侧0.5米的距离。**

- **G线应平行于测试车辆的中心线，并与测试车辆车身最左外边缘左侧3.0米的距离。**

  


**影像方案**

工作速度范围：车辆行驶速度大于10KM/H自动启动

弯道适应性：125m 以上

报警方式：如果目标车辆满足所有条件，则应向驾驶员发出左侧盲点警告（右侧同理）

- 目标车辆的任何部分位于B线前方；
- 目标车辆完全在C线后面；
- 目标车辆完全位于F线左侧；
- 目标车辆的任何部分都在G线的右侧；




分为侧面盲区监测（SBSD）和转向盲区监测（STBSD）

最顶上的线为蓝色，编号为0；最左边的黄色线（即车身线），编号为1；红色线为一级报警区域线，编号为2；中间黄色线为二级报警区域线，编号为3；最右边绿色线为三级报警区域线，编号为4；BSD摄像头固定后，可以通过车载遥控调整车身线以及设置报警区域。

![图片](D:\Yang7hi\note\typora_photo\InterviewBook\686809ad39f1833c2ff87bdc9b915941.jpeg)

采用影像的技术方式，在恶劣天气（大雨、大雾等）下就会表现不佳，极易产生误判。

**雷达方案**

盲点监测系统使用的雷达主要为 24GHz 和 77GHz 的短波雷达

##  ADAS|高级驾驶辅助系统

Driver Monitoring System | 驾驶员状态监测

##  扩展开发控制和数据上下行方面的工作内容

与产品经理和客户沟通，不同的渣土车控制系统的具体需求是去别的，包括车辆状态监测、运行控制以及故障诊断和ADAS等。

CAN（Controller Area Network）总线是一种用于汽车及工业自动化的串行通信协议。其主要特点包括：

- **多主结构**：CAN总线采用多主结构，允许多个节点在同一网络中进行通信，没有主从之分。

- **优先级仲裁**：CAN总线通过消息的优先级进行仲裁，优先级高的消息可以抢占总线，确保重要信息的及时传输。

- **可靠性**：CAN总线具有很强的抗干扰能力和错误检测机制，如CRC校验、错误帧检测和自动重传等。

交通部808协议是一种车辆远程信息管理的标准协议。车载终端通过SIM物联网卡，将数据通过4G/5G网络上传至远程服务器或云平台。通过CAN总线将数据帧发送到车载终端。

## 你在优化终端菜单界面和智慧安卓屏GUI界面时采取了哪些措施？这些措施带来了哪些改进？

OSD叠加中不仅有辅助驾驶信息，实时车速等数据和信息，各个区域的车辆影像。对菜单层级进行重新设计，减少不必要的层级。确保每个摄像头的影像数据都附带正确的标识符。

------



# `WebServer`

## 项目介绍

webserver项目是我学习计算机网络知识和Linux socket编程中开发的轻量级服务器项目。整个网络模型是主从Reactor加线程池的方案。具备处理多个http和ftp协议的能力，同时还能提供轻量化的存储。

最后的压力测试，因为我最后没有部署到云端服务器上，在本地测试下，存储引擎操作的读写QPS都有30多万。webbench

### muduo集群聊天器

使用muduo网络库搭建网络核心模块、Nginx实现聊天服务器的集群，提高并发能力、Redis作为消息中间件、MySQL作为数据存储、json序列化和反序列化作为通信协议的实时聊天服务器。

1. 使用muduo网络库作为项目的网络核心模块，提供高并发网络IO服务，解耦网络和业务模块代码；
2. 使用json序列化和反序列化消息作为私有通信协议；
3. 配置nginx基于tcp的负载均衡，实现聊天服务器的集群功能，提高后端服务的并发能力；
4. 基于redis的发布-订阅功能，实现跨服务器的消息通信；
5. 使用mysql关系型数据库作为项目数据的落地存储；
6. 使用连接池提高数据库的数据存取功能。

![image-20240727174439283](D:\Yang7hi\note\typora_photo\InterviewBook\image-20240727174439283.png)

## 和普通的web服务器的不同改进之处

小顶堆

## 1.阻塞/非阻塞 同步/异步

阻塞和非阻塞、同步和异步都是描述IO的一个状态，一个典型的网络IO包含两个阶段：数据准备（阻塞和非阻塞）和数据读写（同步和异步）。

以一个套接字文件描述符==sockfd==举例，通过==recv()==从已连接的套接字中接受数据，传入==buf==。

- **阻塞模式**

  调用recv()时，数据若没有就绪，recv会阻塞当前线程；

- **非阻塞**

  调用==recv()==时，数据若没有就绪，立即返回==错误码EAGAIN（不同系统可能不同）==，表示操作没法完成，因为数据没准备好。线程可以继续执行其他操作，而不需要等待数据的到达。

  当数据就绪好了，可以再次调用==recv()==读取数据。

- **同步**

  ==recv()==**一直等待数据**，完全复制到缓存区buf，无法执行其他操作；

- **异步**

  允许应用程序在发起IO请求后，可以继续执行操作。当数据拷贝完成时，通过信号或回调==通知==应用程序。

  使用异步IO接口时，调用函数指定`sockfd`和目标缓冲区`buf`;

  操作完成时，sigio信号或者回调**通知**时候buf数据已经拷贝好了。

  ```cpp
  while (true) {
      int nfds = epoll_wait(epollfd, events, MAX_EVENTS, -1);
      for (int i = 0; i < nfds; ++i) {
          if (events[i].events & EPOLLIN) {
              ssize_t bytes_received = recv(events[i].data.fd, buf, sizeof(buf) - 1, 0);
              buf[bytes_received] = '\0';
              std::cout << "Received: " << buf << std::endl;
          }
      }
  }
  ```

## 2.0 Unix/Linux上的5种IO模型

1. **阻塞IO（Blocking I/O）**

   在阻塞IO模型中，当用户程序调用IO操作（如`recv()`）时，如果数据没有准备好，调用会阻塞，线程会等待数据准备好并完成数据拷贝。整个过程中，线程会一直等待，直到操作完成。

2. **非阻塞IO(Non-Blocking I/O)**

   在非阻塞IO模型中，当用户程序调用IO操作（如`recv()`）时，如果数据没有准备好，调用会立即返回一个错误（如`EAGAIN`），而不会阻塞线程。线程可以继续执行其他操作，并在适当的时候再次尝试IO操作。

3. **IO多路复用**

   IO多路复用使用系统调用（如`select()`、`poll()`或`epoll()`）来监视多个文件描述符（如套接字）。当任何一个文件描述符就绪时，系统调用会返回，用户程序可以进行相应的IO操作。

   适合处理多个IO操作，避免了轮询多个文件描述符的开销，但**本质上仍然是同步IO**，因为在就绪事件通知后，用户程序仍需负责数据拷贝。

   **<a href="##epoll是同步的还是异步的">🔗epoll是同步的还是异步的</a>**

   **<a href="##epoll | 为什么使用epoll">🔗epoll，select，poll对比</a>**

4. **信号驱动IO（Signal-Driven I/O）**

   **异步通知，同步数据拷贝**。

   在信号驱动IO模型中，用户程序首先通过设置信号处理程序（如`sigaction()`）来**注册信号处理函数**，并将套接字设置为非阻塞模式和信号驱动模式（使用`fcntl()`设置`O_ASYNC`标志）。

   当数据准备好时，内核会发送一个信号（如`SIGIO`）给用户程序，通知其可以进行IO操作。

5. **异步IO**

   **完全异步**

   在异步IO模型中，用户程序发起IO请求后可以**立即继续执行其他操作**。当IO操作完成（包括数据拷贝）时，内核会通知用户程序（通过信号或回调函数），此时数据已经准备好，用户程序可以直接处理数据。

![image-20240728203028062](D:\Yang7hi\note\typora_photo\InterviewBook\image-20240728203028062.png)

### 总结

**同步IO（阻塞IO、非阻塞IO、IO多路复用、信号驱动的数据拷贝）**：

- **通知方式**：这些模型都使用==就绪事件通知方案==，即通知用户程序数据已经准备好。
- **数据拷贝**：用户程序需要负责将数据从内核缓冲区拷贝到用户缓冲区，或者将数据从用户缓冲区拷贝到内核缓冲区。
- **等待时间**：用户程序需要等待数据拷贝完成，这段时间是同步的。

**异步IO（异步/信号驱动的异步通知）**：

- **通知方式**：通知的是事件完成，即数据已经拷贝完毕，用户程序可以立即进行下一步处理。
- **数据拷贝**：由内核负责完成数据拷贝，用户程序不需要等待数据拷贝时间。
- **效率**：更高，因为用户程序可以在IO操作进行时执行其他任务，最大化利用CPU时间。

## 2.1 IO多路复用技术

一种高效的IO处理方式，允许单个线程同时管理多个IO通道，避免了创建多个线程的开销。网络编程中：使用一个进程来维护监听**多个Socket**的方式。

一个进程虽然任意时刻只能处理一个请求名单时如果每个请求事件的耗时控股之在1ms内，则1s就可以处理上千条请求，把时间拉长看，就是多个请求复用了一个进程，这就是多路复用。这种思想很类似与一个CPU并发多个进程，所以也叫**时分多路复用**。

Linux内核中提供了**`select/poll/epoll`**这三个多路复用的系统调用，进程可以通过一个系统调用函数**从内核中获取多个事件。**

**原理：**`select/poll/epoll`在获取事件时，先把所有的连接（文件描述符，如Socket连接）传给内核，将多个IO事件添加到一个事件集合中进行监听。再由内核返回产生了事件的连接，然后在**用户态**中再处理这些连接对应的请求。

## 2.2 IO多路复用技术|流程

1. 创建Socket文件描述符，并将需要监听的IO事件添加到事件集合中
2. 调用IO多路复用函数，将所有待处理事件的集合传递给函数
3. IO多路复用函数会`等待并监听`**所有传递的事件集合中的IO事件**，并自动挂起当前进程，直到有事件发生或超时
4. 当某个IO事件触发时，IO多路复用函数会返回该事件的文件描述符，并从事件集合中删除该事件
5. 处理完该事件后，将该事件重新加入事件集合中。循环2-5step

## 3.WebServer的作用

一个WebServer就是一个服务器软件。主要功能是通过HTTP协议与客户端（通常是浏览器）进行通信，来接受，存储，处理来自客户端的HTTP请求，并对其请求做出HTTP响应，返回给客户端其请求的内容或返回ERROR信息，最后实现了上万的并发连接。

![image-20240727222355121](D:\Yang7hi\note\typora_photo\InterviewBook\image-20240727222355121.png)

## 4.HTTP协议（应用层协议）

<a href="##HTTP | 超文本传输协议，Hypertext Transfer Protocol">HTTP汇总</a>

## 4. HTTP协议|浏览器地址栏键入URL，按下回车之后会经历什么  

1. 解析URL，生成发给WEB服务器的HTTP请求
2. 查询服务器域名对应的IP地址，如果缓存中有对应域名缓存，就直接返回；没有，向DNS服务器请求解析该URL中域名所对应的IP地址
3. 解析出IP地址后，就可以把HTTP的传输工作交给操作系统的==协议栈==；
4. 根据IP地址和默认端口80，和服务器建立TCP连接；
5. 浏览器发出读写文件（URL中域名后面部分对应的文件）的HTTP请求，该请求作为TCP三次握手的第三个报文数据发送给服务器；
6. 服务器对浏览器请求做出响应，并把对应的资源发送给浏览器
7. 完成以上过程后，数据已经到达浏览器端，接下来浏览器解析并渲染数据
8. 释放TCP连接。

> HTTP协议是基于TCP/IP协议之上的应用层协议，基于**请求-响应**的模式。HTTP协议规定请求从客户端发出，最后服务器端响应该请求并返回。  
>
> 协议栈上半部分是TCP/UDP协议，执行收发数据的操作；下半部分是IP协议，控制网络包的收发

## 6.两种高效的事件处理模式 | Reactor高并发

IO多路复用监听事件，收到事件后，根据事件类型分配给某个进程/线程。有多种的实现方式

![在这里插入图片描述](D:\Yang7hi\note\typora_photo\InterviewBook\e0029a69ac754dea95ef6a81eca5f490.png)

### Reactor模式核心组成部分：

1. **Reactor**：负责监听和分发I/O事件，包括连接事件/读写事件。

2. **处理资源池**：可以是进程或线程，负责实际处理事件。

**方案**

- **单Reactor单进程/线程**：一个Reactor在单个进程或线程中处理所有事件。
- **单Reactor多进程/线程**：一个Reactor分发事件到多个进程或线程。
- **多Reactor多进程/线程**：多个Reactor分布在不同的进程或线程中，每个Reactor处理一部分事件。

其中Reactor负责监听和分发事件；Acceptor负责获取连接；Handler负责处理业务逻辑

### 单Reactor单进程/线程（单单，单线程模型）

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5p-P5rK5,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center-1721222159204-15.png" alt="img" style="zoom:33%;" />

1. Reactor通过IO多路复用接口监听事件，根据事件的类型决定**分发**给Acceptor还是Handler处理；

2. 若是**连接建立**的事件，则交给**Acceptor**对象处理，Acceptor对象会通过accept系统调用来获取连接，并建立一个Handler对象来处理后续的响应事件；

3. 若**不是连接建立事件**，则交由**当前连接对应的Handler**对象来进行响应；

4. Handler对象通过r**ead->业务处理->send**的流程来完成完整的业务流程

- **优点**： 实现简单，无需考虑进程间通信和多进程竞争。
- **缺点**：无法充分利用多核CPU；Handler在进行业务处理时候，整个进程无法连接其他事件。例如，长耗时业务，响应将会延迟。
### 单Reactor多进程/线程（单多，多线程模型）

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5p-P5rK5,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center.png" alt="在这里插入图片描述" style="zoom: 33%;" />

Reactor通过IO多路复用接口监听事件和**单Reactor单进程**（1-3）一致

4. Handler对象不再负责业务逻辑的处理，只负责数据的接受和发送。Handler对象通过**read读取数据**后，会将数据发送给**线程池中子线程里的processor对象**进行业务处理；
5. 子线程里的processor对象处理完后，会将结果发送给**主线程中的Handler对象**，再通过send方法将响应发给client。

- **优点**： 能充分利⽤多核CPU性能 
- **缺点**：一个Reactor承担所有事件的监听和响应，而只在主线程中进行。随间高并发场景中，无法及时处理新连接、就绪的 IO 事件以及事件转发等。

### 多Reactor多进程/线程（多多，主从多线程模型）

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5p-P5rK5,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center-1721222381763-18.png" alt="在这里插入图片描述" style="zoom: 25%;" />

<img src="D:\Yang7hi\note\typora_photo\InterviewBook\主从Reactor多线程.png" alt="多 Reactor 多进程 / 线程方案的示意图" style="zoom: 67%;" />

1. 主线程中的`MainReactor`对象通过IO多路复用接口监听连接建立事件，收到事件后通过Acceptor对象中的accept获取连接，将新的连接分配某个子线程；
2. 子线程中的`SubReactor`对象将MainReactor对象分配的连接加入IO多路复用接口继续监听，并创建一个Handler用于处理连接的响应事件；
3. 如果有新的事件发生，SubReactor通知当前连接对应的Handler对象来进行响应；
4. Handler对象通过`read->业务处理->send`的流程来完成完整的业务逻辑。

- 实现简单，主线程子线程分工明确，主线程子线程交互简单。

## 6.两种高效的事件处理模式 | Reactor和Proactor的好处和坏处  

Reactor 和 Proactor 是两种常用的`并发设计模式`，用于处理 I/O 多路复用，特别是在高性能服务器和网络编程中

- **Reactor 模式**

  **非阻塞同步网络模式**，感知的是就绪可读写的信号：主要用于同步 I/O 操作。它依赖于事件分发器（Event Demultiplexer），如 `select`、`poll` 或 `epoll`，来监视多个 I/O 事件并分发到相应的事件处理程序（Event Handler）。

  - 优点
    1. Reactor实现相对简单，对于耗时短的处理场景处理⾼效  
    2. 简单易懂。Reactor模式更简单易懂，容易实现和维护  
  - 缺点
    1. **单线程瓶颈** 在并发度很⾼场景，`单个线程负责监听和分发所有的IO事件`，可能会成为性能瓶颈。
    2. **同步 I/O 操作阻塞** 处理时间较长的任务可能会阻塞事件处理程序，降低系统响应速度。

- **Proactor 模式**

  **异步网络模式**，感知的是已完成的读写信号：主要用于异步 I/O 操作。它将 I/O 操作的处理从应用程序中解放出来，由**操作系统或 I/O 库完成**，**需要传入数据缓冲区的地址等信息，⽤来存放结果数据** 。当 I/O 读写操作完成时，操作系统会通知应用程序，并将结果传递给相应的事件处理程序。

  - 优点
    1. **异步 I/O 特性** 高并发和高负载情况下，异步 I/O 操作可以`避免阻塞`，提高 I/O 操作的并发度
    2. **处理长时间运行的任务** 如大规模数据传输和文件复制等操作。
  - 缺点
    1. **实现较为困难** 需要处理复杂的异步编程模型
    2. **小规模任务的处理效率略低** 需要进行线程切换和上下文切换，额外的开销可能会影响任务的处理效率

## 6.两种高效的事件处理模式 | 使用Reactor 方案而不是Proactor？为什么不用异步方案？

- Linux 对 Proactor 支持不足，异步 I/O 支持不完善

  **异步 I/O 的局限性** Linux 虽然有 AIO （Asynchronous I/O）接口，这些接口主要用于本地文件系统，`不是在内核级别实现的真正异步 I/O`，网络socket就不适用。

- `epoll` 等高效 I/O 多路复用机制

  **内核支持** Linux 提供了高效的 I/O 多路复用机制，如 `epoll`、`select` 和 `poll`，这些机制在内核级别实现，能够高效地监视大量文件描述符（包括网络 socket）。这些 I/O 多路复用机制非常适合Reactor 模式

- CPU多核心和优秀线程调度算法也可能与异步操作的性能差不多

## 7.线程池|手写线程池



## 7.线程池|线程池和任务队列有没有做分离

有的。主要是为了提升资源的利用率和系统的响应能力，同时也有利于线程安全。**线程池负责生成和管理线程，任务队列用于存储任务**。线程池从任务队列中提取任务，到一个线程中去执行，有任务就执行，无任务阻塞线程休眠。

## 7.线程池|线程池中怎么利用信号量机制

1. **初始化，线程阻塞：** 线程池中的线程通过run方法从任务队列中提取任务，初始化信号量值为0。 由于信号量的初始值为0，在尝试执行任务之前会执行一个等待（wait）操作，这会导致它们阻塞，直到有任务可用。
2. **任务提交**，**信号量增加，唤醒线程：** 线程将任务提交到任务队列，并调用线程池的`append`方法。这个方法将**任务添加到队列中**，执行post做一个**V操作**（通常对应于`sem_post`函数），增加+1信号量，`至少有一个等待的线程会被唤醒`。这个线程会从**阻塞状态转换到就绪状态。**
3. **执行任务：**执行前P操作，减少信号量的值。使得**信号量-1**，然后从任务队列中取出任务执行。
4. **循环等待：**如果信号量的值大于0，线程可以继续从队列中获取新任务；如果信号量的值为0，线程将再次执行等待操作，进入阻塞状态

## 7.线程池|为什么要使用线程池

假设一个 Web 服务器需要处理大量的客户端请求。如果每个请求都创建一个新线程来处理，会导致以下问题：

- 大量线程创建和销毁的开销会影响服务器性能。
- 系统资源可能会被过多的线程耗尽，导致服务器崩溃。

通过使用线程池，服务器可以预先创建一个固定数量的线程来处理请求：

**1）减少线程创建和销毁的开销**  `创建和销毁线程`是⼀项开销较⼤的操作。线程池在应用启动时预先创建一定数量的线程，而不是在每次需要时动态创建和销毁。这减少了系统在创建和销毁线程时的开销，提升了性能。

**2）提高系统资源利用率**  线程池可以通过配置``最大线程数``来限制**并发线程**数量，防止系统资源（例如内存和 CPU）被过多的线程耗尽，从而避免资源竞争和过载。

**3）提高响应性** 当有新任务到达时，线程池中的线程可以⽴即处理它们，⽽不需要等待新线程的创建。线程池中的**线程**在完成一个任务后可以被**复用**

## 7.线程池|线程池怎么保证线程安全

- 使用了互斥锁，通过对共享资源的加锁和解锁来保证并发访问的安全
- 使用信号量，可以用于实现多个线程的同步和互斥
- 还可以加读写锁，条件变量和原子操作来保证线程安全

## 8.有限状态机 (finite state machine)

有限状态机是一种计算模型，它由一组状态以及在这些状态之间的转移组成。在任何给定时间，有限状态机都有一个当前状态，并且基于输入和当前状态，它可以转移到另一个状态或保持在当前状态。HTTP协议的格式和结构之所以适合使用有限状态机进行解析。<a href="###HTTP | HTTP报文">HTTP报文</a>

1. **初始状态（START）**：状态机开始等待接收HTTP请求报文。
2. **请求行状态（REQUEST_LINE）**：当状态机接收到数据时，它首先尝试解析请求行。请求行包含请求方法（如GET、POST等）、URI（统一资源标识符，它指定了请求的资源位置和路径）、以及HTTP协议的版本号。状态机将分析这些组成部分，确保它们符合HTTP协议的规范。
3. **头部状态（HEADERS）**：解析完请求行后，状态机进入头部状态，开始解析请求头。请求头由多个键值对组成，每对之间用CRLF分隔。常见的请求头字段包括Host、User-Agent、Accept等，它们提供了请求的附加信息。状态机将逐行读取并解析这些头部字段。
4. **空行状态**：在请求头解析完毕后，状态机检测到一个空行（两个连续的CRLF），这表示请求头已经结束，请求体（如果有的话）即将开始。
5. **主体状态（BODY）**：对于需要请求体的请求方法（如POST、PUT），状态机进入主体状态并开始解析请求体。请求体可能包含表单数据、JSON对象或其他类型的数据。状态机将根据请求头中的Content-Type字段来确定如何解析这些数据。
6. **结束状态（END）**：一旦请求体被完全解析，或者对于没有请求体的请求（如GET请求），状态机将进入结束状态。在这个阶段，状态机将完成对整个HTTP请求报文的解析，并准备生成相应的响应。
7. **错误状态（ERROR）**：如果在解析请求行、请求头或请求体的过程中遇到任何不符合HTTP协议规范的错误，状态机将进入错误状态。在错误状态下，状态机会记录错误信息，并准备发送一个错误响应给客户端。
8. **处理完成（REQUEST_COMPLETED）**：在结束状态之后，状态机将准备并发送响应给客户端，并返回初始状态（START），等待接收下一个HTTP请求。

整个过程确保了HTTP请求的各个组成部分被逐步解析，并且在每个阶段都有机会处理错误并重新开始。

## 8.有限状态机|主、从状态机调用关系与状态转移过程

**从状态机负责读取报文的一行，主状态机负责对该行数据进行解析**，主状态机内部调用从状态机，从状态机驱动主状态机。

#### **主状态机**

三种状态，标识解析位置。

- CHECK_STATE_REQUESTLINE，解析请求行
- CHECK_STATE_HEADER，解析请求头
- CHECK_STATE_CONTENT，解析消息体，仅用于解析POST请求

#### **从状态机**

三种状态，标识解析一行的读取状态。

- LINE_OK，完整读取一行
- LINE_BAD，报文语法有误
- LINE_OPEN，读取的行不完整

![状态机](D:\Yang7hi\note\typora_photo\InterviewBook\状态机.jpg)

### **从状态机逻辑**

在HTTP报文中，每一行的数据由\r\n作为结束字符，空行则是仅仅是字符\r\n。因此，可以通过查找\r\n将报文拆解成单独的行进行解析，项目中便是利用了这一点。

从状态机负责读取buffer中的数据，将每行数据末尾的\r\n置为\0\0，并更新从状态机在buffer中读取的位置m_checked_idx，以此来驱动主状态机解析。

- 从状态机从m_read_buf中逐字节读取，判断当前字节是否为\r

- - 接下来的字符是\n，将\r\n修改成\0\0，将m_checked_idx指向下一行的开头，则返回LINE_OK
  - 接下来达到了buffer末尾，表示buffer还需要继续接收，返回LINE_OPEN
  - 否则，表示语法错误，返回LINE_BAD

- 当前字节不是\r，判断是否是\n（**一般是上次读取到\r就到了buffer末尾，没有接收完整，再次接收时会出现这种情况**）

- - 如果前一个字符是\r，则将\r\n修改成\0\0，将m_checked_idx指向下一行的开头，则返回LINE_OK

- 当前字节既不是\r，也不是\n

- - 表示接收不完整，需要继续接收，返回LINE_OPEN

### 主状态机逻辑

主状态机初始状态是CHECK_STATE_REQUESTLINE，通过调用从状态机来驱动主状态机，在主状态机进行解析前，从状态机已经将每一行的末尾\r\n符号改为\0\0，以便于主状态机直接取出对应字符串进行处理。

- ==CHECK_STATE_REQUESTLINE==
  - 主状态机的初始状态，调用parse_request_line函数解析请求行
  - 解析函数从m_read_buf中解析HTTP请求行，获得请求方法、目标URL及HTTP版本号
  - 解析完成后主状态机的状态变为CHECK_STATE_HEADER


解析完请求行后，主状态机继续分析请求头。在报文中，==请求头和空行的处理使用的同一个函数，这里通过判断当前的text首位是不是\0字符==，若是，则表示当前处理的是空行，若不是，则表示当前处理的是请求头。

- ==CHECK_STATE_HEADER==
  - 调用parse_headers函数解析请求头部信息

  - 判断是空行还是请求头，若是空行，进而判断content-length是否为0，如果不是0，表明是POST请求，则状态转移到CHECK_STATE_CONTENT，否则说明是GET请求，则报文解析结束。

  - 若解析的是请求头部字段，则主要分析connection字段，content-length字段，其他字段可以直接跳过，各位也可以根据需求继续分析。

  - connection字段判断是keep-alive还是close，决定是长连接还是短连接

  - content-length字段，这里用于读取post请求的消息体长度

GET和POST请求报文的区别之一是有无消息体部分，GET请求没有消息体，当解析完空行之后，便完成了报文的解析。

但后续的登录和注册功能，为了避免将用户名和密码直接暴露在URL中，我们在项目中改用了POST请求，将用户名和密码添加在报文中作为消息体进行了封装。
为此，我们需要在解析报文的部分添加解析消息体的模块。

```cpp
while((m_check_state==CHECK_STATE_CONTENT && line_status==LINE_OK)||((line_status=parse_line())==LINE_OK))
```

在GET请求报文中，每一行都是\r\n作为结束，所以对报文进行拆解时，仅用从状态机的状态line_status=parse_line())==LINE_OK语句即可。

但在POST请求报文中，消息体的末尾没有任何字符，所以不能使用从状态机的状态，这里转而使用主状态机的状态作为循环入口条件。

- ==CHECK_STATE_CONTENT==

- - 仅用于解析POST请求，调用parse_content函数解析消息体
  - 用于保存post请求消息体，为后面的登录和注册做准备


## 9.epoll|为什么使用epoll

Linux内核中提供了**`select/poll/epoll`**这三个多路复用的系统调用

1. **select**：使用固定长度`bitsmap`来表示文件的描述符集合，他支持的文件描述符个数是有限的，在Linux中默认最大值为1024；当然可以更改数量，但是由于select采用**<u>轮询的方式</u>扫描文件描述符**，文件描述符越多，性能越差；**触发方式是水平触发**，应用程序如果**没有完成**对一个已经就绪的文件描述符进行**IO操作**，那么**之后**每次select调用**还是会将文件描述符通知进程**

2. **poll**：使用动态数组存储，以链表形式来组织，突破了select文件描述符的个数限制

**select/poll**并没有本质区别，都是使用线性结构来存储Socket集合。需要遍历文件描述符集合找到可读可写Socket，时间复杂度为O(n)；

同时，都需要在用户态和内核态之间拷贝文件描述符集合；文件描述符越多，损耗成指数增加。都是属于**同步通知**。

3. epoll
   1. **`epoll`** 使用**事件驱动的机制**，**提供了异步通知**，在内核里维护了一个**就绪事件链表**。当某个socket有事件发生时，通过回调函数，内核会将其加入到就绪事件链表中。当用户调用`epoll_wait`函数时，只返回有时间发生的文件描述符的个数，不需要遍历整个集合，大大提高监测效率。

   1. **`epoll`** 在内核中维护了一个**红黑树**，增删改一般时间复杂度是 `O(logn)`。可以保存所有待检测的Socket，所以每次只需要传入一个待检测的Socket，而不需要传入整个Socket，减少了用户空间和内核大量数据拷贝和内存分配


  > `epoll_create` 创建一个` epoll`对象 `epfd`，
  > 通过 `epoll_ctl` 将需要监视的 socket 添加到`epfd`中
  > 调用 `epoll_wait` 等待发生事件`fd`资源
 ![img](D:\Yang7hi\note\typora_photo\InterviewBook\epoll.png)
```cpp
int s = socket(AF_INET, SOCK_STREAM, 0);
bind(s, ...);
listen(s, ...)

int epfd = epoll_create(...);
epoll_ctl(epfd, ...); //将所有需要监听的socket添加到epfd中

while(1) {
    int n = epoll_wait(...);
    for(接收到数据的socket){
        //处理
    }
}
```

## 9.**`epoll`** | epoll中可以无限承载socket的连接吗？创建socket时的返回值是什么  

- epoll本身没有连接数的限制 但是内存是有限的，**1G的内存上能监听约10w个端口**
- 如果Socket创建成功，返回的是一个整数的文件描述符，用于后续的Socket操作。如果创建失败，则会返回`-1`，并且可以使用errno变量来获取具体的错误信息

## 9.**`epoll`** | 边缘触发ET和水平触发LT-

epoll 支持两种事件触发模式，分别是**边缘触发（edge-triggered，ET）和水平触发（level-triggered，LT）**。

**LT（muduo采用）**：内核数据没读完，就会一直上报数据。**服务器端不断地从 epoll_wait 中苏醒，直到内核缓冲区数据被 read 函数读完才结束。**muduo为什么使用LT模式：不会丢失数据；照顾多个连接的公平性，低延迟处理；跨平台使用，ET一些系统不能使用

**ET**：内核数据只上报一次。**服务器端只会从 epoll_wait 中苏醒一次**。

## 9.**`epoll`**  | EPOLLONESHOT | 解决竞态条件和潜在的数据不一致

LT只要存在事件就会不断的触发，ET只在从非触发到触发两个状态转换的时候才触发。

**存在问题：**多线程处理中，一个Socket事件到来，数据开始解析，这时候这个Socket又来一个相同的事件，在前一个解析未完成情况下，程序会自动调用另外一个线程或进程处理新的事件，造成了**不同线程和进程处理同一个socket事件**。

**EPOLLONESHOT解决：**将 socket 文件描述符和 `EPOLLONESHOT` 标志一起注册到 epoll 实例中。这样，当事件发生时，它只会被触发一次。在当前线程完成事件处理后，需要再次使用 `epoll_ctl` 与 `EPOLL_CTL_MOD` 命令来重新注册该 socket 文件描述符，以便它可以对新的事件做出响应。从而保证不会跨越多个线程。

## `fd`在系统中有限制吗？

**有限制的**。

1. 单个进程中默认是`1024`

   ```shell
   ulimit -n	# 查看当前进程的fd数量限制
   ulimit -n num	# 修改当前进程的fd数量限制
   ```

   进程中可以用系统函数修改

   ```cpp
   #include <sys/resource.h>
   struct rlimit {
       rlim_t rlim_cur;	// soft limit
       rlim_t rlim_max;	// hard limit
   };
   // get resource limit
   int getrlimit(int resource, struct rlimit *rlimit);
   // set resource limit
   int setrlimit(int resource, const struct rlimit *rlimit);
   ```

2. 操作系统对文件描述符也有限制，进程分配的文件描述符数量不能超过操作系统的限制，可以通过修改内核参数来调整阈值。文件描述符总是有数量的，取决于系统的配置和硬件资源。

   ```shell
   sysctl fs.file-max = 655360
   ```

## 一个服务端进程最多和多少个客户端进行连接？和fd的数量有关系吗？

文件描述符的数量与同时连接的客户端数量有关，因为每个客户端连接都需要⼀个文件描述符。但是fd并不是唯一影响同时连接客户端数量的因素。

**其他因素**  

- 内存：每一个连接都会消耗一定的内存，内存大小会限制同时连接的客户端数量
- 网络宽带：每个联机的数据传输需要网络宽带
- 处理器性能

## 10.WebBench访问请求原理

- 父进程fork若干个子进程，每个子进程在用户要求或默认时间内对wb循环发起实际访问请求。

- 父子进程通过管道进行通信，子进程通过管道写端向父进程传递若干次请求访问完毕后记录到的总信息

- 父进程通过管道读取子进程发来的相关信息

- 子进程在时间后结束，父进程在所有子进程退出后统计并给用户显示最后结果。退出

  <img src="D:\Yang7hi\note\typora_photo\InterviewBook\image-20240725233235228.png" alt="image-20240725233235228" style="zoom:50%;" />

## 负载均衡

分配网络或者应用的流量到==多个服务器==上，优化资源利用，提高响应速度和增加系统可靠性。

Nginx 默认实现了 **轮询** 和 **最小连接数** 两种负载均衡算法，并支持通过配置实现 **加权轮询** 和 **IP 哈希**。

**见的负载均衡算法包括以下几种：**

是的，常见的负载均衡算法包括以下几种：

1. **轮询（Round Robin）**：
  
   - **描述**：将请求按顺序轮流分配到每个服务器上。每个服务器在处理完一个请求后，下一个请求就会分配到下一个服务器。
   - **优点**：简单易实现，适用于服务器性能相近的场景。**缺点**：不考虑服务器的实际负载或性能差异。

   ```nginx
   upstream backend {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
   }
   ```
   
2. **随机（Random）**==nginx不支持==：
  
   - **描述**：随机选择一台服务器来处理每个请求。这种方法简单且具有一定的负载均衡效果。
   - **优点**：实现简单，负载均衡效果依赖于随机性的良好分布。**缺点**：可能会导致某些服务器过载，而其他服务器闲置。

3. **最小连接数（Least Connections）**：
  
   - **描述**：将请求分配给当前连接数最少的服务器。这有助于确保负载均衡时各服务器处理的请求数量相对均衡。
   - **优点**：更能反映服务器当前的负载状态。**缺点**：需要实时跟踪每个服务器的连接数，可能会增加开销。

   ```nginx
   upstream backend {
       least_conn;
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
   }
   
4. **加权轮询（Weighted Round Robin）**：
   - **描述**：与轮询类似，但每台服务器被赋予一个权重，权重高的服务器会处理更多的请求。
   - **优点**：可以根据服务器的实际能力调整负载分配。**缺点**：需要配置权重参数，可能不适用于服务器能力变化频繁的场景。
   
   ```nginx
   upstream backend {
       server backend1.example.com weight=3;
       server backend2.example.com weight=1;
       server backend3.example.com weight=2;
   }
   ```
   
5. **加权最小连接数（Weighted Least Connections）** ==nginx不支持==：
  
   - **描述**：在最小连接数的基础上考虑服务器的权重，权重高的服务器即使连接数少，也可能会处理更多的请求。
   - **优点**：结合了最小连接数和加权策略的优点，适用于能力差异大的服务器集群。**缺点**：配置复杂，需要合理设定权重。

6. **IP 哈希（IP Hash）**：
  
   - **描述**：根据客户端的 IP 地址计算哈希值，将请求分配到特定的服务器上。相同的 IP 地址会被分配到同一台服务器，从而实现会话保持。
   - **优点**：实现会话保持，确保同一客户端的请求始终由同一台服务器处理。**缺点**：如果服务器数量变化或 IP 地址分布不均，可能会导致负载不均。
   
   ```nginx
   upstream backend {
       ip_hash;
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
   }
   ```

## 在服务端接受accept()之后，socket就是一直可读吗？就是调用read()函数一直可以读吗？会阻塞吗



## Qt信号槽机制的优势和不足

优点：类型安全，松散耦合。缺点：同回调函数相比，运行速度较慢。

---

## 问题01 | **使用优先级队列优化定时器**

**描述：**优先级队列来优化定时器时候，初步构思是使用标准模板库（STL）中的`priority_queue`实现。调整定时器adjust_timer操作的时候，相应需要修改保存的定时器指针内部的expire_time的值。但是这并不会进行自动排序。

**原理**：`priority_queue`不提供直接修改元素值的接口，后面考虑这是因为**`priority_queue`是基于堆实现的**，允许访问或移除队首元素元素，同时排序依赖于元素的值，而直接修改元素值不会触发重新排序。

**解决方法：**选择使用vector数组来重新实现⼀个`小顶堆`，**通过`unordered_map`来保存定时器的下标**。当需要调整指定的定时器的时候，先通过哈希表找到定时器在vector中的位置，然后修改内部的值后，把它移动到合适的位置。  

## 问题02 | 使用`pthread_create`创建线程

**描述：**使用 **pthread_create** 创建线程时，**如果线程函数是类的一个非静态成员函数，会遇到编译错误**

**原理**：**`pthread_create`**函数要求传入的线程函数是一个普通函数，其函数指针类型为`void*(*)(void*)`。这意味着线程函数只能接受一个`void*`类型的参数，而不包括任何额外的参数（如`this`指针）。

- `pthread_create`接受参数包含：指向新线程的标识符指针、线程属性的指针、线程函数的地址和传递给线程函数的参数。

- 线程函数的要求：`pthread_create`要求传入的线程函数是一个普通函数。该函数指针必须接受一个`void*`类型的参数，且该函数返回类型必须是`void*`。

- 非静态成员函数的问题：

  **隐含的`this`指针**：非静态成员函数与普通函数不同，因为它们包含一个隐含的`this`指针，指向调用该函数的对象实例。

  **函数指针不兼容**：由于非静态成员函数需要一个额外的`this`指针参数，因此无法直接转换为`void*(*)(void*)`类型的函数指针。编译器会期望得到一个`this`指针，这导致了与`pthread_create`要求的函数指针类型不匹配，从而导致编译错误。

**解决方法**：

1. 声明线程函数为**静态成员函数**：静态成员函数不隐含`this`指针

```c++
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```

```cpp
// 线程函数，必须是静态的，因为它将被pthread_create调用
static void* threadFunction(void* arg) {
    // 这里可以执行一些线程任务
    std::cout << "Thread with ID " << pthread_self() << " is running." << std::endl;
    return nullptr; // 线程函数应返回void*
}

int main() {
    pthread_t thread_id;
    // 创建线程
    if (pthread_create(&thread_id, NULL, threadFunction, NULL) != 0) {
        std::cerr << "Error creating thread" << std::endl;
        return 1;
    }
    // 等待线程结束
    pthread_join(thread_id, NULL);
    std::cout << "Thread with ID " << thread_id << " finished." << std::endl;
    return 0;
}
```

2. 使用普通函数
3. 使用lambda表达式：捕获对象指针并在其中调用非静态成员函数。

## 问题03 | 解决MySQL `mysql_real_connect` 发生段错误 (核心已转储)

**描述：**在使用Nginx进行负载均衡，并通过Redis集群服务器进行通信的方案中，我们遇到了`mysql_real_connect`方法引发的段错误。Redis中使用独立线程接收订阅通道消息

**Nginx负载均衡 + Redis集群服务器通信**

   1. **Nginx负载均衡**：

      - Nginx作为反向代理服务器，负责将客户端请求分发到不同的后端服务器，以实现负载均衡和高可用性。
      - 通过配置Nginx，可以将请求分发到多个应用服务器，提升系统的处理能力。

   2. **Redis集群服务器通信**：

      - Redis作为高性能的内存数据库，用于在集群服务器之间进行通信和数据共享。

      - 通过Redis的发布/订阅机制，实现消息的广播和处理。

**解决方法**

在调用`mysql_real_connect`时，程序发生段错误并生成核心转储。这通常是由于指针或内存管理问题引起的。

1. **确保`conn_`指针已正确初始化**：
   - 在调用`mysql_real_connect`之前，必须确保`conn_`已经通过`mysql_init`正确初始化。
   2. **检查字符串参数的有效性**：

      - 确保传递给`mysql_real_connect`的字符串参数（例如`server.c_str()`, `user.c_str()`, `password.c_str()`, `dbname.c_str()`）是有效的。


   3. **增加错误检查和日志**：
- 在调用`mysql_real_connect`之前和之后增加详细的日志输出，以便更好地理解问题发生的位置。

**试图访问`reply->element[2]`和`reply->element[2]->str`，但是`reply`可能为`nullptr`，或者`reply->element`数组没有足够的元素**

1.    增加了对`redisReply`对象及其成员的检查：

```cpp
// 在独立线程中接受订阅通道的消息
void Redis::observer_channel_message()
{
    redisReply *reply = nullptr;
    while (REDIS_OK == redisGetReply(this->subcribeContext_, (void **)&reply))
    {
        if (reply != nullptr)
        {
            if (reply->type == REDIS_REPLY_ARRAY && reply->elements >= 3)
            {
                if (reply->element[2] != nullptr && reply->element[2]->str != nullptr)
                {
                    // 给业务层上报通道发生的消息
                    notifyMessageHandler_(atoi(reply->element[1]->str), reply->element[2]->str);
                }
            }
            freeReplyObject(reply);
        }
    }
    cerr << ">>>>>>>>>>>>> observer_channel_message quit <<<<<<<<<<<<<" << endl;
}
```

   > Redis回复的类型包括：
   >
   > 1. **`REDIS_REPLY_STRING`**：回复是一个字符串。
   > 2. **`REDIS_REPLY_INTEGER`**：回复是一个整数。
   > 3. **`REDIS_REPLY_ARRAY`**：回复是一个数组，通常用于订阅消息。
   > 4. **`REDIS_REPLY_NIL`**：回复为空。
   > 5. **`REDIS_REPLY_STATUS`**：回复是一个状态字符串（如命令执行成功的信息）。
   > 6. **`REDIS_REPLY_ERROR`**：回复是一个错误消息。

   **在处理订阅消息时，Redis的回复通常是一个包含三元素的数组：**

      1. 消息类型（如 "message"）
      2. 频道名称
      3. 消息内容
