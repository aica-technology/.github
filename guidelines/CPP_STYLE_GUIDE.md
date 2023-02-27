# AICA C++ Style Guide and Recommendations

* [AICA Style Summary](#AICA-style-summary)
* * [General](#general)
* * [File name and structure](#file-name-and-structure)
* * [General naming](#general-naming)
* * [Format and whitespace](#format-and-whitespace)
* [Automatic formatting](#automatic-formatting)
* * [clang-format](#clang-format-installation-and-usage)
* * [CLion](#clion)
* * [Visual Studio Code](#visual-studio-code)
* * [Sublime Text](#sublime-text)


## Foreword 
C++ is a beast of a language - it is traditional, and it is modern. It is so close, and yet so far, from base C. It lets you get incredibly close to the hardware and manage every bit of memory yourself, and at the same time it provides incredible abstraction patterns.

Within AICA, where people's experience and ambitions with coding projects are diverse, the aim of this page is not to militantly enforce modern, gold-standard programming. Rather, we just want code to be consistent and readable so we can focus on actual research problems.

The [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) has been widely adopted as good and safe practice. [It is not without its flaws](https://eyakubovich.github.io/2018-11-27-google-cpp-style-guide-is-no-good/), and can be quite restrictive particularly towards modern use of the language. Still, its rules are clear and provide a solid foundation for shared code projects.

The [ANYbotics ANYmal Research C++ Style Guide](https://anybotics.github.io/styleguide/cppguide.html) makes a few minor adaptations to the base Google style in the context of robotics research, and is the recommended reference for AICA.

To further promote readability and usability within AICA, some additional minor changes have been suggested below. **Read and adopt the summary below first, then use the ANYbotics guide for everything else.**

Use this guide in conjunction with git best practices and pull request guidelines and aim for incremental improvement to your code quality and usability with each new commit.

All the greatest code is written one line at a time.

## AICA Style Summary

### General

Write code in clear, readable, well-scoped blocks; prefer using multiple specific sub-functions in place of one big function. **Document your code with a README** and inline comments wherever the usage or behaviour is not painfully obvious.

**Keep your code portable** - don't hardcode file paths, environment variables or OS-specific behaviour if it can be avoided. If your code requires external dependencies to run, make sure you also provide installation and build instructions (CMakeLists, Dockerfile, README, etc).

### File name and structure

Source files are `*.cpp`, header files are `*.hpp`. Filenames should be `snake_case` unless they are declaring / implementing a class, in which case the filename should match the class name. 

#### Header files
Header files can use `#pragma once` directive instead of include guards.

The order of `#include` directives should start with built-in standard libraries, then external libraries, then AICA libraries from other modules / packages, and finally other headers from the local package. 

Use quotation marks (`#include "..."`) only for including local packages. Use `#include <...>` everywhere else.

Include only the dependencies necessary for the declarations in the header. Put any further implementation dependencies in the related source file. 

All declarations should be wrapped in a namespace scope where the name in `snake_case` is easily relatable to your package.

An example header file `my_package/my_header_file.hpp` is given below.
```c++
#pragma once

#include <iostream>
#include <memory>

#include <rclcpp/node.hpp>
#include <Eigen/Core>

#include <state_representation/space/cartesian/CartesianState.hpp>
#include <controllers/ControllerFactory.hpp>
#include <other_aica_package/foo.hpp>

#include "my_package/typedefs.hpp"

namespace my_package {

  // declarations go here

}

```

#### Source files
The first `#include` of a source file should be its relevant header file.

Again, code should be wrapped in a namespace scope where the name in `snake_case` is easily relatable to your package. 

An example source file `my_package/my_header_file.cpp` is given below.
```c++
#include "my_package/my_header_file.hpp"

#include <vector>

#include <rclcpp/parameters.hpp>

#include <other_aica_package/bar.hpp>

#include "my_package/implementations.hpp"
#include "my_package/utils.hpp"

namespace my_package {

  // implementations go here

}

```

### General naming

- Type names (including enums, structs, classes, typedefs, etc) should be in `UpperCamelCase`. Virtual or interface classes should use the interface prefix `I`.
- All functions and variables, whether static or local, should be in `lower_snake_case`, with the following exceptions; class private and protected data members should have an underscore suffix: `lower_snake_case_`. 
- When using enums, use `UPPER_SNAKE_CASE` for the members. 

```c++
enum MyEnum {
  DEFAULT = 0,
  SECOND_CASE,
  OTHER
};

struct MyStruct {
  int public_data_member;
};

class IMyVirtualClass {
public:
  virtual void my_virtual_function() = 0;
};

class MyClass : public IMyVirtualClass {
public:
  void my_virtual_function() override;
  float get_private_data() const;
  float public_data_member;
protected:
  float protected_data_member_;
private:
  float private_data_member_;
};

void my_function(const MyClass& readonly_reference, MyClass& mutable_reference) {
  int my_local_variable;
}
```


### Format and whitespace

- Indents are 2 spaces.
- Conditionals and loops always have brackets, with `else if` / `else` statements following the closing bracket on the same line. No spaces inside parentheses.
- In a `for` loop, place a space after `;`, and on both sides of `:`.
- `switch` statements should define a default case. Only use additional scoping brackets within a case when necessary (e.g. declaring a local variable).

```c++
if (condition) {
  foo();
} else if {
  bar();
} else {
  ...
}

while (condition) {
  foo();
}

for (int i = 0; i < 10; ++i) {
  foo();
}

std::vector<SomeType> range(...);
for (auto iter : range) {
  bar();
}

switch (value) {
  case 0:
    foo();
    break;
  case 1: {
    float local_var;
    bar();
    break;
  }
  default:
  case 2:
    foo();
    bar();
    break;
}
```

Pointer and reference operators `*` and `&` should follow the type. 
```c++
int a = 0;
int* b = &a;
int& c = a;
int& d = *b;
```

Prefer smart pointers to raw pointers whenever possible.

## Automatic formatting

While it is useful to internalize the rules of this style guide while writing your code, it would be cumbersome and unrealistic to check every line against every rule manually. Fortunately, the majority of the formatting rules can be applied to your code automatically. By configuring your development environment to automatically format your code (for example, format on save), you can ensure that each new addition you make will conform to the lab standard without any effort on your part. 

Some files have been provided to help automatically reformat your code according to the AICA style. Use the AICA style directly within your code editor by importing the [IntelliJ IDEA code style XML](https://github.com/epfl-AICA/wiki/blob/master/files/AICA%20IntelliJ%20IDEA%20CPP%20Code%20Style.xml). Alternatively, post-format your code using your editor or the `clang-format` command-line tool with the [clang-format-AICA file](https://github.com/epfl-AICA/wiki/blob/master/files/clang-format-AICA).

Follow the setup instructions below and within less than 5 minutes, your favourite code editor will automatically format your code in the future.

_(Instructions written for Ubuntu 18.04)_

### CLion
Download the [IntelliJ IDEA code style XML file](./AICA_IDEA_CPP_STYLE.xml) from this repository.

In CLion, go to `File` &rarr; `Settings` &rarr; `Editor` &rarr; `Code Style`. Then, under `Scheme`, click on `Show Scheme Actions` and then `Import Scheme...`. Select the file you downloaded and confirm with `OK`. 

You can use the `Reformat Code` or `Reformat File...` actions to apply the style rules as needed. Use `Optimize imports` to re

If using the CLion VCS window to commit code, you can enable automatic formatting before each commit. Go to `Version Control` &rarr; `Commit` and select `Reformat code` and `Optimize imports` under `Before Commit`.

Optionally, you can also enable automatic formatting whenever the code is saved. Go to `Tools` &rarr; `Actions on Save` and select `Reformat code` and `Optimize imports`.

## Authors / Maintainers
- Enrico Eberhard ([enrico@aica.tech](mailto:enrico@aica.tech))
- Dominic Reber ([dominic@aica.tech](mailto:dominic@aica.tech))