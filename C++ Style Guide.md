#### 功能和性能相关

1. 首先考虑使用前置自增自减

2. 避免不需要的隐式转换，请使用 explicit 声明构造函数

3. 请使用 DISALLOW_COPY_AND_ASSIGN 声明空的拷贝构造函数和赋值操作。隐式拷贝在 C++ 中导致很多性能问题和 bug，大量类可以不必拷贝，但是编译器默认提供 public 的拷贝构造函数和赋值操作。这个定义请使用 `base/common.h` 头文件。

   ```
   class Foo {
   public:
     Foo(int f);
     ~Foo();
   private:
     DISALLOW_COPY_AND_ASSIGN(Foo);
   };
   ```

4. 避免使用多重继承，如果必须使用，请确保继承中除了父类其余均为纯接口。

5. 避免使用 using 污染命名空间，除了在函数、方法或类等局部情况。

6. 头文件引用尽量使用前置声明，减少 .h 文件中 #include 的数量。

7. 全局变量：不要实用class类型的全局变量，如string，vector，如有需要，使用类指针或者单例模式

8. 类的指针成员变量尽量使用智能指针，当类对象作为函数参数且不需要修改时，尽量使用const &

#### 命名约定

1. 函数名、变量名、文件名等尽量使用描述性名称，不要节约空间，以读懂为主，不要随意使用不清晰的字符或缩写。

2. 文件命名全部小写，使用下划线（_）增加可读性。头文件以 .h 结尾，C++ 文件以 cc 结尾，如 `spdy_interface.h`  和 `spdy_interface.cc`。

3. 类型命名（类、结构体、类型定义、枚举）使用驼峰方式定义，不包含下划线。接口名称尽量以 interface 结尾。

4. 变量命名

   - 普通变量一律小写，以下划线作为连接符，类成员变量使用下划线结尾，结构体数据成员和普通变量一致。

   - 全局变量使用 g_ 开头，其余与普通变量一致。

   - 常量命名使用 k 开头，其余字母使用驼峰形式，没有下划线。

   - ```
     // 普通变量
     string table_name;
     
     // 类数据成员使用下划线结尾
     class Foo {
         string name_;
     }
     
     // 结构体数据成员和普通变量一致
     struct UrlTableProperties {
         string name;
         int num_entries;
     }
     
     // 全局变量使用g_开头，其与普通变量一致
     static char* g_name;
     
     // 常量命名使用 k 开头，其余字母使用驼峰形式，不使用下划线
     const int kDaysInAWeex;
     ```

5. 函数命名

   - 普通函数以大写字母开头，其余单词以驼峰形式，没有下划线。

   - 存取函数与存取的变量名匹配，均为小写字母，使用下划线连接。如果存取函数中不仅是存取作用则命名方式与普通函数一致。另外存取函数一般内联在头文件。

   - 获取成员数据的函数需要const来修饰，在const函数里有必要修改的成员变量使用mutable修饰

   - ```
     // 普通函数
     AddTableEntry();
     
     // 存取函数
     class MyClass {
       public:
         ...
         int num_entries() const { used_count_++; return num_entries_; }
         void set_num_entries(int num_entries) { num_entries_ = num_entries; }
       
       private:
         int num_entries_;
         mutable int used_count_;
     }
     ```

6. 命名空间均使用小写，命名尽量基于目录结构。

7. 枚举值全部大写，单词之间以下划线相连。

8. 宏定义均以大写和下划线组成。

   ```
   #define ROUND(x) ...
   #define PI_ROUNDED 3.0
   ```

9. 头文件引用时使用全路径引用（root 为源码根路径），系统的使用尖括号，其余的大部分使用双引号,这么做可以从代码中清晰了解依赖的模块，以及在编译过程中尽量减少include文件夹。

   ```
   #include <string>
   #include "file/base/file.h"
   #include "third_party/skia/include/core/SkBitmap.h"
   ```

#### 格式

1. 函数尽可能短小、紧凑、功能单一

2. 复杂逻辑函数尽量写完整注释，增加可读性

3. 预处理指令不要缩进

4. cpp文件静态函数集中写在类函数前面

5. 头文件引用的顺序是：C 库、C++库、其他库的 .h、项目内的 .h

   ```
   #include <map>
   #include <string>
   #include <vector>
   
   #include "base/compiler_specific.h"
   #include "base/memory/scoped_ptr.h"
   #include "net/spdy/buffered_spdy_framer.h"
   ```

#### 设计模式

1. A模块需要使用B模块的输出，尽量使用client模式（A是B的客户）来维持两个模块的独立
2. 不属于A模块逻辑操作，而需要A 模块触发的，尽量使用代理模式，保持模块代码和功能的独立

