```cpp
#include <pybind11/embed.h> // 必须包含 embed.h

#include <iostream>

namespace py = pybind11;

int main() {
    py::scoped_interpreter guard{}; // 初始化 Python 解释器

    try {
        // 导入 Python 模块 'exp' (即 exp.py 文件)
        py::module_ exp_module = py::module_::import("exp");

        // 调用 Python 函数 hello_from_python
        py::object hello_function = exp_module.attr("hello_from_python");
        py::object python_result = hello_function(); // 调用函数并获取返回值
        std::string cpp_result = python_result.cast<std::string>(); // 将 Python 对象转换为 C++ 字符串
        std::cout << "Python 函数 hello_from_python 返回: " << cpp_result << std::endl;

        std::cout << "-------------------------" << std::endl;

        // 调用 Python 函数 add_numbers
        py::object add_function = exp_module.attr("add_numbers");
        int sum_result = add_function(10, 20).cast<int>(); // 传递参数并获取整数返回值
        std::cout << "Python 函数 add_numbers(10, 20) 返回: " << sum_result << std::endl;


    } catch (const py::error_already_set &e) {
        // 捕获并打印 Python 异常
        std::cerr << "Python 错误发生: " << e.what() << std::endl;
        return 1; // 返回错误代码
    }

    return 0; // 成功返回
}
```