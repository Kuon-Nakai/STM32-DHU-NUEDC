# X-1 关于ARM Compiler 6 (AC6)

ARM Compiler 6 (以下简称AC6) 是ARM公司推出的一款编译器，其编译速度和代码质量都比较优秀，也对C++有更好的支持，但是与很多代码库的兼容性不是很好。在此记录一些使用AC6的经验。

## 切到AC6之后我的工程出现了很多编译错误，怎么办？

别急 你先别急 先看文档

> 以下部分基于: Keil关于转向使用AC6的文档 <https://www.keil.com/appnotes/files/apnt_298.pdf>

关于如何切换到AC6 可以看第2页的图片

### 切换到AC6的前提

- Keil版本 5.23以上，我们提供的版本是5.28
- 软件包 Keil MDK-Middleware Pack v7.4.0及以上
- 软件包 Keil ARM Compiler Support Pack v1.3.0及以上
- 软件包 ARM CMSIS Pack v5.0.1及以上

以上三个软件包都已经默认安装。

### 不兼容的语言拓展

参见第4页表格。第一列为AC5的表达方式，第二列为AC6，第三列为官方推荐的CMSIS方式。

由于两个编译器的语言include包是独立的，最底层的库不会出现这类问题。

### 不兼容的pragma


