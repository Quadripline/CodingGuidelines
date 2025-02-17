# C++ 代码命名规范（C++20） v1.0-alpha

---

## 1. 文件与文件夹命名（File and Folder Names）
- **文件名**  
  - 采用首字母大写形式，例如：  
    - `MyClass.cpp` / `MyClass.h`  
    - `GameEngine.cpp` / `GameEngine.h`  
- **文件夹名**  
  - 同样采用首字母大写形式或与文件名一致的风格。  
  - 例如：`ProjectFiles`、`CoreModules`、`CustomWidgets` 等。
- **模块文件（Modules，`.ixx` 等）**  
  - 若使用 C++20 模块特性，可将模块文件命名为 `ModuleName.ixx`，仍然推荐首字母大写（如 `MyModule.ixx`）。
  - 模块命名（module `MyModule`）本身也建议采用与类名类似的 PascalCase 风格。

---

## 2. 函数命名（Function Names）
- 采用首字母大写（PascalCase）形式。  
- 例子：  
  ```cpp
  int GetCount();
  void ProcessData();
  ```

---

## 3. 变量命名（Variable Names）
- 采用**驼峰式（camelCase）**，首字母小写，后续单词首字母大写。  
- 例子：  
  ```cpp
  int itemCount;
  float maxSpeed;
  std::string userName;
  ```

---

## 4. 常量与宏定义（Constants & Macros）
- **全大写**并使用下划线连接单词。  
- 对于常量，可以使用 `constexpr` 或 `const` 修饰的写法：  
  ```cpp
  constexpr int MAX_BUFFER_SIZE = 1024;
  const float DEFAULT_VOLUME = 0.8f;
  ```
- 宏定义也采用相同的全大写下划线方式：  
  ```cpp
  #define DEBUG_MODE
  #define ERROR_CODE_INVALID_INPUT 1001
  ```

---

## 5. 代码块风格（Braces Style）
1. **单行代码块**可以使用行内花括号：  
   ```cpp
   if (condition) { DoSomething(); }
   ```
2. **多行代码块**的 `{` 放在上一行的末尾，`}` 独占一行。示例：  
   ```cpp
   if (condition) {
       // This is a multi-line block
       DoSomething();
       DoSomethingElse();
   }
   else {
       // Another multi-line block
       HandleError();
   }
   ```

---

## 6. 其余常见 C++ 规范与建议（Other Common C++ Guidelines）
1. **缩进（Indentation）**  
   - 建议使用 4 空格进行缩进。  

2. **命名空间（Namespaces）**  
   - 可以使用小写或合适的大小写风格，但要保持一致。示例：  
     ```cpp
     namespace myapp {
         // ...
     }
     ```

3. **类命名（Class Names）**  
   - 与函数命名类似，采用首字母大写（PascalCase），如 `MyClass`, `GameEngine`。

4. **成员变量（Member Variables）**  
   - 同一般变量命名，采用驼峰式，但部分团队会在成员变量前加 `m_` 前缀。此处不做强制，仅需保证与全局变量、局部变量等区分。  
   - 示例：  
     ```cpp
     class Player
     {
     public:
         Player();
         ~Player();

         void SetName(const std::string& name);

     private:
         std::string playerName; // 或 m_playerName
     };
     ```

5. **指针和引用（Pointers & References）**  
   - 声明指针或引用时，`*` 或 `&` 紧随类型。例如：  
     ```cpp
     int* ptr;
     float& ref = value;
     ```

6. **函数参数（Function Parameters）**  
   - 参照变量命名规则，使用驼峰式，必要时使用 `const` 引用或指针来避免不必要的拷贝。  

7. **注释（Comments）**  
   - 代码内的注释建议使用英文，保持简洁明了。可使用 `//` 或 `/* */`。  
   - 使用 Doxygen 风格（如 `///`）时，也建议英文描述，方便自动化文档工具生成。  

8. **auto 的使用**  
   - C++20 中可使用 `auto` 推导类型，但应在语义清晰、易读的情况下使用，避免过度依赖 `auto` 造成可读性下降。  
   - 当类型复杂或推断结果不明显时，建议显式声明类型。

---

## 7. C++20 特性使用建议

### 7.1 Concepts（概念）
- **命名**：建议与类命名类似，采用首字母大写（PascalCase）来定义 Concept 名称。  
  ```cpp
  template <typename T>
  concept Integral = std::is_integral_v<T>;
  ```
- **使用场景**：对于需要明确约束类型的模板接口，优先使用 Concepts，而不是传统的 SFINAE 或 `std::enable_if`。

### 7.2 Ranges & Views（范围与视图）
- 当使用 `std::ranges` 相关功能时，变量命名依旧采用驼峰式。  
- 保持管道操作（|）语义可读，不建议在一行中拼接过多视图造成可读性下降，可适当换行：
  ```cpp
  auto results = data
               | std::views::filter([](auto x) { return x > 0; })
               | std::views::transform([](auto x) { return x * 2; });
  ```

### 7.3 Coroutines（协程）
- 对于协程函数（`co_await`, `co_yield`, `co_return`）的命名与普通函数相同，仍然遵守函数首字母大写（PascalCase）的原则：
  ```cpp
  task<int> FetchDataAsync()
  {
      co_return 42;
  }
  ```
- 为了标识异步语义，可以在函数名中增加 `Async`、`Awaitable` 等后缀，但并非强制：
  ```cpp
  task<void> ProcessDataAsync()
  {
      // ...
      co_await SomeOperation();
      co_return;
  }
  ```

### 7.4 Modules（模块）
- **文件命名**：使用 `ModuleName.ixx`、`MyLibrary.ixx` 等首字母大写形式。  
- **模块声明**：在模块头部声明时，与文件名一致或相近：
  ```cpp
  module MyLibrary;
  ```
- **导入导出**：建议在模块文件顶部先行导入依赖模块，然后进行 `export`，清晰地组织依赖结构。  

### 7.5 编译指示（Attributes）
- C++20 中新增了如 `[nodiscard]` 等属性。保留标准拼写即可，无需特殊命名。  
  ```cpp
  [[nodiscard]] int ComputeValue();
  ```

---

## 8. 参考示例（Sample Code）
下面是一段示例代码，展示 C++20 特性与上述规范的结合使用：

```cpp
// FileName: SampleModule.ixx
module SampleModule;

import <iostream>;
import <string>;
import <vector>;

// Concept with PascalCase
template <typename T>
concept StringLike = requires(T a)
{
    // We only check if a has .size() and operator[]
    { a.size() } -> std::convertible_to<std::size_t>;
    { a[0] } -> std::convertible_to<char>;
};

// Exported function with PascalCase
export int CountUpperCase(StringLike auto&& str)
{
    int count = 0;
    for (char ch : str) {
        if (std::isupper(static_cast<unsigned char>(ch))) {
            ++count;
        }
    }
    return count;
}
```

**要点解析：**  
- 模块文件名 `SampleModule.ixx`：首字母大写。  
- `StringLike` concept：PascalCase 命名。  
- `CountUpperCase`：函数名首字母大写。  
- 变量 `count`、`ch`：采用驼峰式（或更简短的单词命名）。  

---

## 9. 注意事项
- 本规范在 **C++20** 基础之上总结了一些常见风格与命名建议，不同团队可根据实际需求进行微调。  
- 对于已有代码或第三方库，不要求强制改动，但新增或重构部分应尽量遵循此规范。  
- 编译器与构建系统可能对模块、协程等特性有不同实现或要求，请结合实际环境进行配置。

---

**总结**  
以上风格包含了常规 C++ 命名约定，以及对 C++20 新特性的部分命名与组织建议。通过统一文件、文件夹、函数、变量、常量、概念 (Concept)、模块 (Module) 等方面的命名和使用规则，可以让团队开发在协同与代码审阅中更加高效、可维护。  
