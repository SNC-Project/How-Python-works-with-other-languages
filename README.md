# How-Python-works-with-other-languages

## English

# Python Interfacing with Other Languages

Python allows seamless integration with various other programming languages. Below are the key methods for interfacing Python with C, C++, Java, and Rust, using representative tools and libraries for each.

---

## 1. Python + C Language

**Primary Methods**: Ctypes and Cython

### **Ctypes**
`ctypes` is a Python standard library that loads and uses dynamically linked libraries (`.dll` on Windows or `.so` on Linux) written in C.

**C Code (example.c):**
```
#include <stdio.h>

void say_hello() {
    printf("Hello from C!\n");
}
```

**Compile Command** (Linux):
```
gcc -shared -o example.so -fPIC example.c
```

**Using in Python:**
```
import ctypes

# Load the C function
lib = ctypes.CDLL('./example.so')
lib.say_hello()
```

### **Cython**
`Cython` allows you to write C-like code that is compiled into a Python module, making the integration seamless.

**Cython Code (example.pyx):**
```
def add(int a, int b):
    return a + b
```

**Setup File (setup.py):**
```
from setuptools import setup
from Cython.Build import cythonize

setup(
    ext_modules=cythonize("example.pyx")
)
```

**Compile Command:**
```
python setup.py build_ext --inplace
```

---

## 2. Python + C++

**Primary Methods**: Pybind11 and Boost.Python

### **Pybind11**
Pybind11 allows you to call C++ code from Python with minimal boilerplate code and supports modern C++11 syntax.

**C++ Code (example.cpp):**
```
#include <pybind11/pybind11.h>

namespace py = pybind11;

int add(int a, int b) {
    return a + b;
}

PYBIND11_MODULE(example, m) {
    m.def("add", &add, "A function that adds two numbers");
}
```

**Compile Command:**
```
c++ -O3 -Wall -shared -std=c++11 -fPIC $(python3 -m pybind11 --includes) example.cpp -o example$(python3-config --extension-suffix)
```

**Using in Python:**
```
import example
print(example.add(3, 4))  # Output: 7
```

---

## 3. Python + Java

**Primary Methods**: JPype and Py4J

### **JPype**
JPype allows you to start a JVM within Python and access Java classes and methods directly.

**Python Code Example:**
```
import jpype

# Start JVM
jpype.startJVM(classpath=['path/to/your/java/classes'])

# Load a Java class
String = jpype.JClass("java.lang.String")
java_string = String("Hello from Java")
print(java_string.toUpperCase())

jpype.shutdownJVM()
```

### **Py4J**
Py4J allows Python code to access Java objects in a running JVM, often used in big data frameworks like Apache Spark.

**Python Code Example:**
```
from py4j.java_gateway import JavaGateway

# Start Java Gateway
gateway = JavaGateway()
random = gateway.jvm.java.util.Random()
print(random.nextInt(10))  # Calls Java's Random function
```

---

## 4. Python + Rust

**Primary Methods**: PyO3 and Rust FFI

### **PyO3**
PyO3 allows you to write Rust code that can be compiled into a Python module, supporting seamless integration.

**Rust Code (lib.rs):**
```
use pyo3::prelude::*;

#[pyfunction]
fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[pymodule]
fn example(py: Python, m: &PyModule) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(add, m)?)?;
    Ok(())
}
```

**Compile Command:**
```
maturin develop
```

**Using in Python:**
```
import example
print(example.add(2, 3))  # Output: 5
```

### **Rust FFI**
With Rust FFI, you can use `#[no_mangle]` and `extern "C"` to create a C-style interface that can be accessed by Python’s `ctypes`.

---



## Korean


# Python과 다른 언어 간의 연동 방법

Python은 여러 언어와 통합하여 사용할 수 있는 다양한 방법을 지원합니다. C, C++, Java, Rust에 대해 각각의 대표적인 연동 방법을 아래에 정리했습니다.

---

## 1. Python + C 언어

**대표 방법**: Ctypes와 Cython

### **Ctypes**
`ctypes`는 Python 표준 라이브러리로, C 언어에서 작성된 동적 라이브러리(`.dll`, `.so`)를 로드하고 사용할 수 있습니다.

**C 코드 작성 (example.c):**
```
#include <stdio.h>

void say_hello() {
    printf("Hello from C!\n");
}
```

**컴파일** (Linux):
```
gcc -shared -o example.so -fPIC example.c
```

**Python 코드에서 호출:**
```
import ctypes

# C 함수 불러오기
lib = ctypes.CDLL('./example.so')
lib.say_hello()
```

### **Cython**
`Cython`은 C 코드를 Python과 비슷한 문법으로 작성하여 Python 모듈로 컴파일하는 방법입니다.

**Cython 파일 (example.pyx):**
```
def add(int a, int b):
    return a + b
```

**Cython 컴파일 (setup.py):**
```
from setuptools import setup
from Cython.Build import cythonize

setup(
    ext_modules=cythonize("example.pyx")
)
```

**컴파일 명령:**
```
python setup.py build_ext --inplace
```

---

## 2. Python + C++

**대표 방법**: Pybind11과 Boost.Python

### **Pybind11**
Pybind11은 Python에서 C++ 코드를 쉽게 호출할 수 있게 해주는 라이브러리입니다. C++11 이상의 문법을 지원합니다.

**C++ 코드 (example.cpp):**
```
#include <pybind11/pybind11.h>

namespace py = pybind11;

int add(int a, int b) {
    return a + b;
}

PYBIND11_MODULE(example, m) {
    m.def("add", &add, "A function that adds two numbers");
}
```

**컴파일 명령:**
```
c++ -O3 -Wall -shared -std=c++11 -fPIC $(python3 -m pybind11 --includes) example.cpp -o example$(python3-config --extension-suffix)
```

**Python에서 사용:**
```
import example
print(example.add(3, 4))  # 출력: 7
```

---

## 3. Python + Java

**대표 방법**: JPype와 Py4J

### **JPype**
JPype는 JVM을 직접 시작하여 Python 코드에서 Java 클래스를 인스턴스화하고 메서드를 호출할 수 있게 해줍니다.

**Python 코드 예제:**
```
import jpype

# JVM 시작
jpype.startJVM(classpath=['path/to/your/java/classes'])

# Java 클래스 불러오기
String = jpype.JClass("java.lang.String")
java_string = String("Hello from Java")
print(java_string.toUpperCase())

jpype.shutdownJVM()
```

### **Py4J**
Py4J는 Python 코드에서 JVM으로 연결된 Java 객체를 호출할 수 있게 하는 라이브러리입니다.

**Python 코드 예제:**
```
from py4j.java_gateway import JavaGateway

# Java Gateway 시작
gateway = JavaGateway()
random = gateway.jvm.java.util.Random()
print(random.nextInt(10))  # Java의 Random 함수 호출
```

---

## 4. Python + Rust

**대표 방법**: PyO3와 Rust FFI

### **PyO3**
PyO3는 Rust 코드를 Python에서 사용할 수 있게 해주는 라이브러리입니다. `maturin` 또는 `setuptools-rust`로 패키징할 수 있습니다.

**Rust 코드 (lib.rs):**
```
use pyo3::prelude::*;

#[pyfunction]
fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[pymodule]
fn example(py: Python, m: &PyModule) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(add, m)?)?;
    Ok(())
}
```

**컴파일**:
```
maturin develop
```

**Python에서 사용:**
```
import example
print(example.add(2, 3))  # 출력: 5
```

### **Rust FFI**
Rust에서 `#[no_mangle]`과 `extern "C"`를 사용하여 C 스타일의 인터페이스를 제공하고, Python에서는 `ctypes`를 통해 불러올 수 있습니다.



