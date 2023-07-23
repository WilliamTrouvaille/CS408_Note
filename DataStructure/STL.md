# STL常用

## std::vector

`std::vector` 是 C++ STL（标准模板库）中的容器之一，它提供了动态数组的功能。`std::vector` 可以存储一系列相同类型的元素，并能够在运行时动态地调整大小。使用 `std::vector` 可以方便地管理和操作动态数组，而无需手动处理内存分配和释放。

以下是 `std::vector` 的详细说明：

- 头文件：`#include <vector>`
- 定义：`std::vector` 是一个类模板（class template），因此可以容纳任何 C++ 类型作为其元素类型。
- 创建对象：可以通过以下方式来创建 `std::vector` 对象：
  ```cpp
  std::vector<数据类型> vectorName;               // 创建一个空的 vector 对象
  std::vector<数据类型> vectorName(size);         // 创建指定大小的 vector 对象
  std::vector<数据类型> vectorName(size, value);  // 创建指定大小并初始化为 value 的 vector 对象
  std::vector<数据类型> vectorName = {val1, val2, ...};  // 利用初始化列表创建 vector 对象
  ```
- 常用成员函数和操作：
  - `push_back(value)`：向 vector 尾部添加一个元素。
  - `pop_back()`：移除 vector 尾部的一个元素。
  - `size()`：返回 vector 中的元素数量。
  - `empty()`：检查 vector 是否为空，返回布尔值。
  - `clear()`：清空 vector，移除所有元素。
  - `operator[]`：访问 vector 中的元素，使用索引访问，如 `vectorName[i]`。
  - `at(index)`：访问 vector 中的元素，带有边界检查。
  - `begin()` 和 `end()`：返回指向 vector 起始和结束位置的迭代器。
  - `resize(newSize)`：改变 vector 的大小为 newSize，如果 newSize 大于原来的大小，会用默认值填充新元素。
  - `erase(position)`：移除 vector 中指定位置的元素。
  - `erase(start, end)`：移除 vector 中指定范围的元素。
  - `insert(position, value)`：在 vector 中指定位置插入一个元素。
  - `insert(position, start, end)`：在 vector 中指定位置插入另一个迭代器范围内的元素。
- 特性：
  - 动态大小：`std::vector` 可以动态调整其大小，可以在运行时动态添加或移除元素。
  - 随机访问：`std::vector` 支持使用索引直接访问元素，因此可以在常量时间复杂度内进行随机访问。
  - 连续内存：`std::vector` 内部使用连续的内存空间存储元素，因此在某些情况下可能需要重新分配内存以扩展容量，导致移动元素的开销。
  - 动态增长：`std::vector` 会在元素数量超过当前容量时自动扩展容量，一般情况下，当容量不足时，会分配更大的内存块并将元素复制到新内存中。

示例：
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    numbers.push_back(6);  // 添加元素 6 到 vector 尾部
    numbers.pop_back();    // 移除 vector 尾部的元素
    numbers.resize(3);     // 缩小 vector 大小为 3

    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
输出：`1 2 3`

- 排序：`std::vector` 支持使用算法进行排序，可以使用 `std::sort` 或 `std::stable_sort` 对其进行排序。示例见前面的回答。

- 插入效率：在 `std::vector` 尾部进行插入和删除操作（`push_back` 和 `pop_back`）的时间复杂度是平均常数时间 O(1)。但是在 `std::vector` 中间或头部进行插入和删除操作时，会导致后面的元素移动，时间复杂度为 O(N)。因此，如果需要频繁在中间或头部进行插入和删除操作，可能会有性能开销，此时可以考虑使用其他容器（例如 `std::list` 或 `std::deque`）。

- 内存管理：`std::vector` 会自动处理内存分配和释放，无需手动管理内存。它会自动增加内部容量以适应元素的增长，并在不再需要时自动释放内存。

- 迭代器失效：在向 `std::vector` 中添加或删除元素时，可能会导致指向元素的迭代器失效。因为插入和删除操作可能导致内存重新分配，迭代器可能会指向已经释放的内存位置。因此，在进行插入和删除操作后，需要小心使用之前的迭代器，最好在操作后重新获取迭代器。

示例：
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // 在 vector 中间插入元素
    numbers.insert(numbers.begin() + 2, 10);

    // 在 vector 头部插入元素
    numbers.insert(numbers.begin(), 20);

    // 在 vector 尾部插入元素
    numbers.insert(numbers.end(), 30);

    // 删除 vector 中间的元素
    numbers.erase(numbers.begin() + 3);

    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
