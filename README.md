# T1 LED发光实验
## 实验信息
- 提交Git编号：待本次提交完后填写
- 芯片：STM32F103C8T6（蓝药丸开发板）
- 软件：STM32CubeMX + STM32CubeIDE

## 实验功能
选用HSE外部高速晶振作为PLL输入源，PLL输出PLLCLK作为系统时钟SYSCLK，实现PA5引脚LED每500ms闪烁一次。系统时钟参数：
- SYSCLK：72MHz
- HCLK：72MHz
- PCLK1：36MHz
- PCLK2：72MHz

## 工程搭建步骤
1. 打开STM32CubeMX，新建工程，选择芯片STM32F103C8T6。
2. 引脚配置：将PA5设置为GPIO_Output推挽输出；SYS中Debug选择Serial Wire，开启SWD调试，用于ST-Link下载。
3. 时钟配置：启用HSE外部高速晶振，配置PLL倍频，选择PLLCLK作为系统时钟，SYSCLK设置为72MHz，保存配置。
4. 生成工程：工具链选择STM32CubeIDE，取消勾选Generate Under Root。
5. 在main.c的while(1)主循环区域，编写LED电平翻转代码。
6. 代码编写完成，编译工程，排查并修复语法错误。

## 源代码运行方法
1. 使用STM32CubeIDE打开工程文件夹。
2. ST-Link仿真器连接开发板与电脑，开发板上电。
3. 编译代码，确认0个错误。
4. 使用STM32CubeProgrammer将程序烧录到单片机。
5. 下载完成，开发板自动复位运行，LED每隔500ms闪烁。

## 遇到问题与解决方案
1. 编译报错：`too few arguments to function 'HAL_GPIO_TogglePin'`
原因：最初只填写PA5，HAL库`HAL_GPIO_TogglePin`函数需要端口、引脚两个入参。
解决：修改代码为`HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);`

2. 宏定义书写错误，误写为`GPIO-PIN-PA5`
原因：C语言里减号属于运算符，HAL库宏定义使用下划线`_`。
解决：更正宏为`GPIO_PIN_5`

## 文件说明
led-blink源码文件夹：完整STM32工程（.ioc时钟与引脚配置文件、源码）
README.md：本次实验记录文档
