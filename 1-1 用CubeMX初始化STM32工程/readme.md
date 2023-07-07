# 1-1 用CubeMX初始化STM32工程

> 本节我们将介绍利用STM32CubeMX(以下简称为Cube)初始化最简单的STM32工程的方法。

## 1. 选择芯片型号

一般我们由芯片型号来创建一个对应的工程。在Cube的初始界面, 我们可以进入MCU Selector来选择芯片型号。

在选择界面中，可以在左侧进行型号搜索或由硬件配置进行筛选。同时右侧会显示备选型号以及详细信息。选择好型号后，点击Start Project即可。如不确定一块开发板的型号，可以直接阅读STM32核心IC上的型号。

加载完毕后，可以看到芯片及其各个引脚，在左侧可以配置各个功能和参数。

## 2. 系统基本配置

> 这些内容在每个Cube项目中都**必须**配置，否则可能导致芯片锁死等问题。

### 1. 系统Debug配置

   在左侧的System Core中，选择SYS，进入系统配置界面。在右侧的Debug中，选择Serial Wire，即SWD模式。忘记配置该项会导致芯片自锁，无法下载程序。

### 2. 系统时钟输入配置

    在单片机中，时钟信号是最重要的信号，它决定了单片机的运行速度。在STM32中，有多种时钟源，如内部高速时钟(HSI)、外部高速晶振(HSE)、低速外部晶振(LSE)等。

   一般我们会使用外部高速晶振(HSE)来作为系统时钟源，因此在左侧的System Core中，选择RCC(Reset and Clock Controller)，进入系统时钟配置界面。在右侧的High Speed Clock中，选择Crystal/Ceramic Resonator，即可启用外部高速晶振输入。

    而低速外部晶振(LSE)一般用于RTC(Real Time Clock)模块，用于实时计时。配置方法与上述相同。我们会在后续的RTC章节中详细介绍，现在暂时不会用到。

### 3. 时钟树配置

   在最上方的选项卡中选择Clock Configuration，进入系统时钟树配置界面。

   在时钟树中，可以理解为每条线为一个时钟信号线，有着固定的频率。这些信号线由时钟源出发，频率经过乘、除，可以连接到右侧的各个模块，如CPU、总线、外设等。

   我们希望使用HSE作为系统时钟源。一般的方法是在PLL Source Mux选择HSE，在System Clock Mux选择PLLCLK。蓝色的PLL区域实现的是对HSE信号的倍频和分频，可以帮助我们把有限频率的HSE信号转换为更高的频率，供系统使用。同时我们一般点击Enable CSS，使能时钟源失效检测，以防止外部晶振失效导致系统崩溃。

   Cube可以实现时钟树自动配置，因此我们并不需要手动调整每个参数。只需指定HCLK频率，即CPU运行频率，即可自动配置各个时钟信号线的频率。

   一般来说为了较高的程序运行速度，我们会选择将HCLK配置为最大值。输入频率并按Enter确认后，Cube会询问是否要解决时钟树的问题，选择Yes即可开始自动配置。

### 4. 项目配置

   在左侧的Project Manager中，选择Project Settings，进入项目配置界面。

- 在右侧的Project Name中，输入项目名称。

- 在Project Location中，选择项目存放的路径。

- 在Toolchain/IDE中，选择所用的IDE。如使用Keil，选择MDK-ARM。如使用VSCode或CubeIDE，选择STM32CubeIDE。

接下来配置Code Generator。这些选项会影响Cube生成的工程代码的结构。

- 最上方的三个选项中一般选择Copy only necessary library files。这样可以减少工程代码的体积，同时维持一定的可移植性。
- 在文件生成选项中，选择Generate peripheral initialization as a pair of '.c/.h' files per peripheral。这样可以将不同外设的初始化代码分离到不同文件中，方便阅读和修改。
- Keep user code while re-generating: 务必勾选，否则重新生成后会清除用户代码。

在完成所有其他配置后，点击Generate Code即可生成工程初始化代码。

> 每次配置新的工程，都必须完成以上步骤。务必牢记 SYS -> RCC -> 时钟树配置。

---

> Author: Shiki
>
> Updated: 2023/7/7
