# Reaction: 现代 C++ 响应式编程框架

[🇨🇳 中文文档](README.zh-CN.md) | [🇬🇧 English](README.md)

[![C++20](https://img.shields.io/badge/C++-20-blue.svg)](https://en.cppreference.com/w/cpp/20)
[![仅头文件](https://img.shields.io/badge/Header--only-是-green.svg)](https://en.wikipedia.org/wiki/Header-only)
[![CMake](https://img.shields.io/badge/CMake-3.15+-blueviolet.svg)](https://cmake.org)
[![响应式](https://img.shields.io/badge/响应式-编程-ff69b4.svg)](https://reactivex.io)
[![模板元编程](https://img.shields.io/badge/模板-元编程-orange.svg)](https://en.cppreference.com/w/cpp/language/templates)
[![MVVM](https://img.shields.io/badge/模式-MVVM%2FMVC-9cf.svg)](https://en.wikipedia.org/wiki/Model–view–viewmodel)

Reaction 是一个极速的现代 C++20 仅头文件响应式框架，将 React/Vue 风格的数据流引入原生 C++ —— 完美适用于 **UI 数据流**、**游戏逻辑**、**金融服务**、**实时计算**等场景。

## 🚀 性能优化

- 通过编译时计算实现**零成本抽象**
- 智能变更传播带来**最小运行时开销**
- 传播效率达到**每秒数百万次**级别

## 🔗 智能依赖管理

- 自动**有向无环图检测**和循环预防
- 细粒度的**变更传播控制**
- 优化**重复依赖导致的多次调用**

## 🛡️ 安全保障

- 使用 C++20 概念进行编译时**类型检查**
- 整个框架采用安全的**值语义**
- 框架内部管理节点生命周期

## 🔄 多线程支持

Reaction 支持多线程，具有自动线程安全检测和单线程模式零开销。

---

## 🔍 对比: `QProperty` vs `RxCpp` vs `Reaction`

| 特性 / 指标               | 🟩 **QProperty (Qt6)**               | 🟨 **RxCpp**                             | 🟥 **Reaction**                          |
| ------------------------ | ------------------------------------ | ---------------------------------------- | ---------------------------------------- |
| **表达式支持**           | ✅ `setBinding()`，但仅**单层**       | ✅ 支持链式 `map`、`combine_latest` 等    | ✅✅ 完全支持**深层嵌套表达式**            |
| **表达式嵌套深度**       | ❌ 限制为一层                        | ⚠️ 支持嵌套，但冗长                     | ✅ 无限深度，自动依赖跟踪                 |
| **更新传播**             | 每层手动传播                        | 每层响应式推送链                        | 基于 DAG 的自动传播，带剪枝               |
| **依赖跟踪**             | ❌ 手动                              | ⚠️ 通过操作符链手动                     | ✅ 通过惰性求值自动捕获依赖               |
| **性能(更新延迟)**       | ✅ 快速 (O(1) 传播)                  | ❌ 慢 (堆分配和嵌套链式)                | ✅✅ 快速 (剪枝更新、惰性求值、差异比较)   |
| **内存使用**             | ✅ 非常低 (栈 + 信号)                | ❌ 高 (大量堆分配的观察者)              | ⚠️ 中等 (DAG 存储，小对象优化)           |
| **语法简洁性**           | ✅ 简单 (`setBinding`, `value()`)    | ❌ 冗长的模板语法                        | ✅ 接近自然语法的表达式模板               |
| **类型支持**             | ✅ 内置类型和注册的自定义类型        | ✅ 基于模板，支持任何类型                | ✅ 支持任何组合的类型擦除或模板           |
| **容器支持**             | ✅ 可用于容器                        | ✅ 可组合多个观察者                      | ✅ 支持容器表达式 (如 map/filter 输出)    |
| **线程模型**             | 默认 UI 线程，信号需手动保证安全     | ✅ 多线程管道                           | ✅ 默认主线程，可插拔锁策略               |
| **错误处理**             | ❌ 无                                | ✅ 健壮的错误流 (`on_error_resume_next`) | ✅ 错误节点传播，可插拔失败策略           |
| **可调试性**             | ⚠️ Lambda 较难追踪                  | ❌ 复杂类型导致困难                     | ✅ 可追踪依赖、可观察 ID、链式跟踪        |
| **模板实例大小**         | ✅ 小                                | ❌ 巨大 (模板爆炸)                      | ✅ 类型擦除或实例去重优化                 |
| **构建时间**             | ✅ 快速                              | ❌ 大型表达式非常慢                     | ✅ 分离头文件，可控实例化                 |
| **学习曲线**             | ✅ 低 (Qt 风格用法)                  | ❌ 陡峭 (函数式风格)                    | ⚠️ 中等 (需理解类型推导+表达式设计)      |
| **适用场景**             | UI 属性绑定，轻量状态同步           | 异步管道，流逻辑                        | UI + 状态建模 + 复杂逻辑的表达式树        |

---


## 📦 需求

- **编译器**: 兼容 C++20 (GCC 10+, Clang 12+, MSVC 19.30+)
- **构建系统**: CMake 3.15+

## 🛠 安装

---

## 1. 从源码构建（手动安装）

要手动构建并安装 **reaction** 响应式框架：

```bash
git clone https://github.com/lumia431/reaction.git && cd reaction
cmake -S . -B build
cmake --install build/ --prefix /your/install/path
```

安装完成后，你可以在自己的基于 CMake 的项目中这样使用 reaction：

```cmake
find_package(reaction REQUIRED)
target_link_libraries(your_target PRIVATE reaction)
```

---

## 2. 使用 vcpkg（推荐方式）

你也可以通过 **vcpkg** 安装 reaction，vcpkg 会自动处理依赖和 CMake 集成。

### 安装 reaction

```bash
cd /path/to/vcpkg
./vcpkg install reaction
```

### 将 vcpkg 集成到你的 CMake 项目

在配置项目时指定 vcpkg 提供的 toolchain 文件：

```bash
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=/path/to/vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build build
```

### 在 CMakeLists.txt 中使用 reaction

```cmake
find_package(reaction CONFIG REQUIRED)

add_executable(my_app main.cpp)

# 链接 vcpkg 提供的目标
target_link_libraries(my_app PRIVATE reaction::reaction)
```

### 卸载

要卸载框架:

```bash
cmake --build build/ --target uninstall
```

如需运行示例或测试单元:

```bash
cmake -S . -DBUILD_EXAMPLES=TRUE -DBUILD_TESTS=TRUE -B build
cmake --build build/
```

## 🚀 快速开始

```cpp
#include <reaction/reaction.h>
#include <iostream>
#include <iomanip>
#include <cmath>

int main() {
    using namespace reaction;

    // 1. 股票价格的响应式变量
    auto buyPrice = var(100.0).setName("buyPrice");      // 股票买入价
    auto currentPrice = var(105.0);                      // 当前市场价格

    // 2. 使用 'calc' 计算盈亏金额
    auto profit = calc([&]() {
        return currentPrice() - buyPrice();
    });

    // 3. 使用 'expr' 计算盈亏百分比
    auto profitPercent = expr(std::abs(currentPrice - buyPrice) / buyPrice * 100);

    // 4. 使用 'action' 在值变化时打印日志
    auto logger = action([&]() {
        std::cout << std::fixed << std::setprecision(2);
        std::cout << "[股票更新] 当前价格: $" << currentPrice()
                  << ", 盈亏: $" << profit()
                  << " (" << profitPercent() << "%)\n";
    });

    // 模拟价格变动
    currentPrice.value(110.0).value(95.0);  // 股票价格上涨
    buyPrice.value(90.0);                   // 调整买入价

    return 0;
}
```

## 📖 基础用法

### 1. 响应式变量: `var`

使用 `var<T>` 定义响应式状态变量。

```cpp
auto a = reaction::var(1);         // int 变量
auto b = reaction::var(3.14);      // double 变量
```

- 获取值:

```cpp
auto val = a.get();
```

- 赋值:

```cpp
a.value(2);
```

### 2. 派生计算: calc

使用 **calc** 基于一个或多个 var 实例创建响应式计算。

- Lambda 捕获风格:

```cpp
auto a = reaction::var(1);
auto b = reaction::var(3.14);
auto sum = reaction::calc([=]() {
    return a() + b();  // 使用 a() 和 b() 获取当前值
});
```

- 参数绑定风格 (高性能):

```cpp
auto ds = reaction::calc([](auto aa, auto bb) {
    return std::to_string(aa) + std::to_string(bb);
}, a, b);  // 依赖: a 和 b
```

### 3. 声明式表达式: expr

expr 提供简洁语法声明响应式表达式。当任何依赖变量变化时，结果自动更新。

```cpp
auto a = reaction::var(1);
auto b = reaction::var(2);
auto result = reaction::expr(a + b * 3);  // 当 'a' 或 'b' 变化时，result 自动更新
```

### 4. 响应式动作: action

注册 action 在观察变量变化时执行动作。

```cpp
int val = 10;
auto a = reaction::var(1);
auto dds = reaction::action([&val]() {
    val = a();
});
```

当然，为了高性能可以使用参数绑定风格。

```cpp
int val = 10;
auto a = reaction::var(1);
auto dds = reaction::action([&val](auto aa) {
    val = aa;
}, a);
```

### 5. 响应式结构体字段

对于复杂类型，响应式字段允许您定义类似结构体的变量，其成员是单独响应式的。

以下是 `PersonField` 类的示例:

```cpp
class PersonField : public reaction::FieldBase {
public:
    PersonField(std::string name, int age):
        m_name(reaction::field(name)),
        m_age(reaction::field(age)){}

    std::string getName() const { return m_name.get(); }
    void setName(const std::string &name) { m_name.value(name); }
    int getAge() const { return m_age.get(); }
    void setAge(int age) { m_age.value(age); }

private:
    reaction::Var<std::string> m_name;
    reaction::Var<int> m_age;
};

auto p = reaction::var(PersonField{"Jack", 18});
auto action = reaction::action(
    []() {
        std::cout << "动作触发, 姓名 = " << p().getName() << " 年龄 = " << p().getAge() << '\n';
    });

p->setName("Jackson"); // 动作触发
p->setAge(28);         // 动作触发
```

### 6. 复制和移动语义支持

```cpp
auto a = reaction::var(1);
auto b = reaction::var(3.14);
auto ds = reaction::calc([]() { return a() + b(); });
auto ds_copy = ds;
auto ds_move = std::move(ds);
EXPECT_FALSE(static_cast<bool>(ds));
```

### 7. 重置节点和依赖

reaction 框架允许您通过替换计算函数来**重置计算节点**。
当节点初始创建后需要使用不同逻辑或不同依赖重新计算结果时，此机制非常有用。

``注意:`` **返回值类型不能改变**

以下是演示重置功能的示例:

```cpp
TEST(TestReset, ReactionTest) {
    auto a = reaction::var(1);
    auto b = reaction::var(std::string{"2"});
    auto ds = reaction::calc([]() { return std::to_string(a()); });
    ds.reset([=]() { return b() + "set"; });

    v.value("3");
    EXPECT_EQ(ds.get(), "3set");

    EXPECT_THROW(ds.reset([=]() { return a(); }), std::runtime_error);  // 返回类型改变
    EXPECT_THROW(ds.reset([=]() { return ds(); }), std::runtime_error); // 循环依赖
}
```

### 8. 触发模式

`reaction` 框架支持多种触发模式来控制响应式计算的重新求值时机。此示例展示三种模式:

1. **值变更触发:** 仅当底层值实际发生变化时触发响应式计算。
2. **过滤触发:** 当值满足指定条件时触发响应式计算。
3. **总是触发:** (此示例未明确展示) 无论值是否变化都触发。

触发模式可通过类型参数指定

```cpp
using namespace reaction;
auto stockPrice = var(100.0);
auto profit = expr(stockPrice() - 100.0);   // 默认 ChangeTrigger
auto assignAction = action<AlwaysTrig>([=]() {
    std::cout << "检查赋值, 价格 = " << stockPrice() <<'\n';
});
auto sellAction = action<FilterTrig>([=]() {
    std::cout << "卖出时机, 盈亏 = " << profit() <<'\n';
});
sellAction.filter([=]() {
    return profit() > 5.0;
});
stockPrice.value(100.0); // assignAction 触发
stockPrice.value(101.0); // assignAction, profit 触发
stockPrice.value(106.0); // 全部触发

```

您甚至可以在代码中自定义触发模式，只需包含 **checkTrig** 方法:

```cpp
struct MyTrig {
    bool checkTrig() {
        // 执行某些操作
        return true;
    }
};
auto a = var(1);
auto b = expr<MyTrig>(a + 1);
```

### 9. 无效策略

在 `reaction` 框架中，用户获取的所有数据源**实际上都是以弱引用形式存在**，其实际内存**由观察者映射管理**。
用户可以手动调用 **close** 方法，这样所有依赖的数据源也会关闭。

```cpp
auto a = reaction::var(1);
auto b = reaction::var(2);
auto dsA = reaction::calc([=]() { return a(); });
auto dsB = reaction::calc([=]() { return dsA() + b(); });
dsA.close(); //dsB 会自动关闭，因为 dsB 依赖 dsA。
EXPECT_FALSE(static_cast<bool>(dsA));
EXPECT_FALSE(static_cast<bool>(dsB));
```

然而，对于用户获取的弱引用生命周期结束的场景，`reaction` 框架为不同场景提供了多种策略。

- **直接关闭策略:**
  当任何依赖变为无效时，节点立即关闭(变为无效)。

- **保持计算策略:**
  节点继续重新计算，其依赖正常工作。

- **最后值策略:**
  节点保留最后有效值，其依赖使用该值进行计算。

以下是展示所有三种策略的简明示例:

```cpp
{
    auto a = var(1);
    auto b = calc([]() { return a(); });
    {
        auto temp = calc<AlwaysTrig, CloseHandle>([]() { return a(); });
        b.set([](auto t) { return t; }, temp);
    }
    // temp 生命周期结束，b 也会结束。
    EXPECT_FALSE(static_cast<bool>(b));
}
{
    auto a = var(1);
    auto b = calc([]() { return a(); });
    {
        auto temp = calc<AlwaysTrig, KeepHandle>([]() { return a(); }); // 默认为 KeepHandle
        b.set([](auto t) { return t; }, temp);
    }
    // temp 生命周期结束，b 不受影响。
    EXPECT_TRUE(static_cast<bool>(b));
    EXPECT_EQ(b.get(), 1);
    a.value(2);
    EXPECT_EQ(b.get(), 2);
}
{
    auto a = var(1);
    auto b = calc([]() { return a(); });
    {
        auto temp = calc<AlwaysTrig, LastHandle>([]() { return a(); });
        b.set([](auto t) { return t; }, temp);
    }
    // temp 生命周期结束，b 使用其最后值计算。
    EXPECT_TRUE(static_cast<bool>(b));
    EXPECT_EQ(b.get(), 1);
    a.value(2);
    EXPECT_EQ(b.get(), 1);
}
```

同样，您可以在代码中自定义策略，只需包含 **handleInvalid** 方法:

```cpp
struct MyHandle {
    void handleInvalid() {
        std::cout << "无效" << std::endl;
    }
};
auto a = var(1);
auto b = expr<AlwaysTrig, MyHandle>(a + 1);
```

### 10. 响应式容器

**Reaction** 支持标准 stl 容器的响应式版本 (`vector, list, set, map` 等)。

```cpp

using namespace reaction;
constexpr int STUDENT_COUNT = 5;
// 1. 学生成绩容器 - 使用 vector 存储 VarExpr
std::vector<Var<double>> grades;
for (int i = 0; i < STUDENT_COUNT; ++i) {
    grades.push_back(create(70.0 + i * 5));
}
// 2. 成绩统计容器 - 使用 list 存储 CalcExpr
std::list<Calc<double>> stats;
stats.push_back(create([&] {
    double sum = 0;
    for (auto &grade : grades)
        sum += grade();
    return sum / grades.size();
}));
stats.push_back(create([&] {
    double max = grades[0].get();
    for (auto &grade : grades)
        max = std::max(max, grade());
    return max;
}));
// 3. 成绩变更监视器 - 使用 set 存储 Action
std::set<Action<>> monitors;
for (int i = 0; i < STUDENT_COUNT; ++i) {
    monitors.insert(create([i, &grades] {
        std::cout << "[监视器] 学生 " << i << " 成绩更新: " << grades[i]() << "\n";
    }));
}
// 4. 成绩等级映射 - 使用 map 存储 CalcExpr
std::map<int, Calc<const char *>> gradeLevels;
for (int i = 0; i < STUDENT_COUNT; ++i) {
    gradeLevels.insert({i, create([i, &grades] {
                            double g = grades[i]();
                            if (g >= 90) return "A";
                            if (g >= 80) return "B";
                            if (g >= 70) return "C";
                            return "D";
                        })});
}

```

### 11. 批量操作

**Reaction** 允许将多个响应式更新分组为单个**批次**，延迟传播直到批次结束。
这有助于消除冗余的中间更新，并在同时更新多个响应式节点时提高性能。

```cpp
using namespace reaction;

Var<int> a = create(1);
Var<int> b = create(2);

// 创建依赖于 a 和 b 的计算值
Calc<int> sum = create([&] {
    std::cout << "[重新计算] sum = " << a() << " + " << b() << "\n";
    return a() + b();
});

// 1. 无批处理: 触发两次重新计算
a.value(10);  // [重新计算] sum = 10 + 2
b.value(20);  // [重新计算] sum = 10 + 20

// 2. 有批处理: 仅触发一次重新计算
auto batchScope = batch([&] {
    a.value(100);
    b.value(200);
});
batchScope.execute();       // [重新计算] sum = 100 + 200

// 或直接使用 'batchExecute' 执行
batchExecute([&] {
    a.value(300);
    b.value(400);
});                         // [重新计算] sum = 300 + 400
```
