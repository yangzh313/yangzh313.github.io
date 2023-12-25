- 要使用CGO，需要安装C/C++构建工具链，在linux安装GCC，在windows则是MinGW。需要保证go环境变量CGO_ENABLED=1，默认开启。
- ### import "C"语句
	- Go代码中出现import "C"语句则表示使用了CGO特性，紧跟在这行语句前面的注释是一种特殊语法，里面包含的是正常的C语言代码。当确保CGO启用的情况下，还可以在当前目录中包含C/C++对应的源文件。
- ### "cgo"语句
	- 这个代码片段是一个在 Go 中使用 CGO（C Go）工具时的特殊注释，用于指定链接时的一些标志（LDFLAGS）。下面是对这句代码的解释：
	- ```
	  #cgo LDFLAGS: -L/usr/local/lib64/protector/ -liscsi -g -Wl,--allow-multiple-definition
	  ```
	- -
	- **`-L/usr/local/lib64/protector/`：** 这个部分指定了链接器在 `/usr/local/lib64/protector/` 目录中查找库文件的路径。 `-L` 是告诉链接器添加库搜索路径的选项。
	- -
	- **`-liscsi`：** 这个部分指定了链接器链接的库文件的名字。在这个例子中，链接器会尝试在指定的库路径中查找 `libiscsi` 库文件。
	- -
	- **`-g`：** 这个部分添加了调试信息。`-g` 选项告诉链接器生成调试信息，这对于在调试过程中更好地追踪源代码和变量非常有用。
	- -
	- **`-Wl,--allow-multiple-definition`：** 这个部分是传递给链接器的一个选项。 `-Wl` 表示将后面的参数传递给链接器，`--allow-multiple-definition` 是链接器的一个选项，允许多次定义，即允许有多个相同的符号定义在不同的目标文件中。这在一些特殊情况下可能是必要的，但通常情况下并不推荐使用，因为它可能引发一些问题，例如链接时的符号冲突等。
	-