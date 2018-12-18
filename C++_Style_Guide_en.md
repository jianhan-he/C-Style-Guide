#### Functional and Performance

1. Use prefix form (`++i`) of the increment and decrement operators with iterators and other template objects.

2. Do not define implicit conversions. Use the `explicit` keyword for conversion operators and single-argument constructors.

3. Use DISALLOW_COPY_AND_ASSIGN to make the class is not copyable or movable. There are many bugs and issue of performance because of implicit copyable . In most cases the class is not need to copyable, but the copy/move constructors can be implicitly invoked by the compiler. If you want to use DISALLOW_COPY_AND_ASSIGN, please include  base/common.h.

   ```
   class Foo {
   public:
     Foo(int f);
     ~Foo();
   private:
     DISALLOW_COPY_AND_ASSIGN(Foo);
   };
   ```

4. Forbid  a sub-class inherits from multiple base classes. If you must use it, please ensure interface is pure abstract .

5. Avoid to use  using namespaces except in some local cases, for example some cases of  function、method and class.

6. Try to use forward declarations to decrement #include files.

7. Global Variables: Try to avoid initialze variable of class type, for example string, vector. If you want to use this way, please to use clase pointer or single portotype.

8. Try to use smart pointer wrap the member of class . If the object param of function can not modify, please use const &

#### Naming convention

1. Give as descriptive a name as possible, within reason. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader.Do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word

2. Filenames should be all lowercase and can include underscores (`_`)。C++ files should end in `.cc` and header files should end in `.h`,e.g., `spdy_interface.h` and `spdy_interface.cc`。

3. Type names start with a capital letter and have a capital letter for each new word, with no underscores.

   Interface names end with interface.

4. Variable Name

   - The names of variables (including function parameters) and data members are all lowercase, with underscores between words. Data members of classes (but not structs) additionally have trailing underscores. Data members of structs, both static and non-static, are named like ordinary nonmember variables.

   - All global variables should start with g_ 

   - Variables declared constexpr or const, and whose value is fixed for the duration of the program, are named with a leading "k" followed by mixed case. Underscores can be used as separators in the rare cases where capitalization cannot be used for separation

   - ```
     // common variable
     string table_name;
     
     // data members of classes
     class Foo {
         string name_;
     }
     
     // data members of classes structs
     struct UrlTableProperties {
         string name;
         int num_entries;
     }
     
     // global variable
     static char* g_name;
     
     // constant named with a leading "k" 
     const int kDaysInAWeex;
     ```

5. Function Names

   - Common functions should start with a capital letter and have a capital letter for each new word, and Underscores can't be used

   - Accessors and mutators (get and set functions) may be named like variables, are all lowercase, with underscores between words.

   - The function of get member data should add "const", if some variables should be modified, please add "mutable"

   - ```
     // common function
     AddTableEntry();
     
     // get and set functions
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

6. Namespace names are all lower-case. Top-level namespace names are based on the project name 

7.  Enumerators should be named with all capitals and underscores.

8. Macro names should be named with all capitals and underscores.

   ```
   #define ROUND(x) ...
   #define PI_ROUNDED 3.0
   ```

9. All of a project's header files should be listed as descendants of the project's source directory without use of UNIX directory shortcuts

   ```
   #include <string>
   #include "file/base/file.h"
   #include "third_party/skia/include/core/SkBitmap.h"
   ```

#### Formatting

1. The function is as short, compact and single as possible

2. Complex logic functions try to write complete comments to increase readability

3. Do not indent the preprocessing instructions

4. The static function should put the head of class on cpp file

5. The order of contained headers: C lib、c++ lib、other lib .h 、project .h

   ```
   #include <map>
   #include <string>
   #include <vector>
   
   #include "base/compiler_specific.h"
   #include "base/memory/scoped_ptr.h"
   #include "net/spdy/buffered_spdy_framer.h"
   ```

#### Design Patterns

1. The A module needs to use the output of the B module, try to use the client mode (A is the customer of B) to maintain the independence of the two modules.
2. Does not belong to the A module logic operation, but needs the A module to trigger, try to use the proxy mode, keep the module code and function independent

