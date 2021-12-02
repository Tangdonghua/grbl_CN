![GitHub Logo](https://github.com/gnea/gnea-Media/blob/master/Grbl%20Logo/Grbl%20Logo%20250px.png?raw=true)

***
_点击 `Release` 页签下载编译好的 `.hex` 文件 或 [点击这里](https://github.com/gnea/grbl/releases)_
***
Grbl是性能高，成本低，基于并口运动控制，用于CNC雕刻。这个Grbl版本运行在携带了328p处理其的Arduino上（例如Uno,Duimilanove,Nano,Micro等）。控制器由C编写并优化，利用了AVR 芯片的每一个灵巧特性来实现精确时序和异步控制。它可以保持超过30kHz的稳定、无偏差的控制脉冲。它接受标准的G代码而且通过了数个CAM工具的输出测试。弧形、圆形和螺旋的运动都可以像其他一些基本G代码命令一样完美支持。函数和变量并不支持，但是我们认为GUI可以更好的完成工作。 Grbl 包含完整的前瞻性加速度控制。它意味着控制器将提前16到20个运动来规划运行速度，以实现平稳的加速和无冲击的转弯。

* [许可证](https://github.com/gnea/grbl/wiki/Licensing): Grbl是自由软件, 在 GPLv3 许可证下发布。

* 更多信息或帮助, 查看我们的WiKi **[WiKi页面!](https://github.com/MillerRen/grbl/wiki)** 如果上面的信息过时了, 请编辑它保持最新并和我们沟通! 谢谢!

* 核心开发者: Sungeun "Sonny" Jeon, Ph.D. (USA) aka @chamnit

* Simen Svale Skogsrud (挪威)编写并创建了神奇的 Grbl v0.6 (2011) 固件。

***

### Grbl CNC 项目的官方支持者
![官方支持者们](https://github.com/gnea/gnea-Media/blob/master/Contributors.png?raw=true)


***

## v1.1版本更新概要
- **重要:** 你的EEPROM将会被擦除并恢复成新设置。 这是由于增加了两个新的主轴转速设置'$'。

- **实时覆盖** : 通过进给、快速、主轴转速、主轴停止和冷却液切换控制，立即改变机器运行状态。这个令人激动的新功能通常只会出现在工业机器上，用来优化运行状态下优化速度和给料。大多数爱好CNC试图模仿这种行为，但通常有大量的滞后。 Grbl能在几十毫秒内执行实时覆盖。

- **手动模式** : 新的手动命令独立于G代码解析器，因此如果没有正确恢复解析器状态，解析器状态不会发生改变，也不会导致潜在的崩溃。  文档包含了这是怎么工作的和它怎么能用控制杆和手轮控制你的机器 文档包含了这是如何工作的，它可以用来控制您的机器通过一个操纵杆或旋转拨号与低延迟，达到令人满意的响应。

- **激光模式** : 新的“激光”模式将使Grbl连续移动通过连续的G1、G2和G3命令和主轴转速变化。当“激光”模式禁用时，Grbl将转而停止，以确保主轴达到适当的速度。主轴速度overrides也与激光模式工作，所以你可以在工作期间调整激光功率。“激光”和“普通”模式可以通过`$`设置切换。

	- **按速度比例调整激光功率** : 如果你的机器有比较小的加速度，Grbl将自动根据行进多快自动调整激光功率，因此当你的CNC转弯时不会烧焦拐角!当激光模式开启时启用`M4`主轴逆时针转动命令开启这个特性！

- **Sleep Mode** : Grbl may now be put to "sleep" via a `$SLP` command. This will disable everything, including the stepper drivers. Nice to have when you are leaving your machine unattended and want to power down everything automatically. Only a reset exits the sleep state.

- **Significant Interface Improvements**: Tweaked to increase overall performance, include lots more real-time data, and to simplify maintaining and writing GUIs. Based on direct feedback from multiple GUI developers and bench performance testing. _NOTE: GUIs need to specifically update their code to be compatible with v1.1 and later._

	- **New Status Reports**: To account for the additional override data, status reports have been tweaked to cram more data into it, while still being smaller than before. Documentation is included, outlining how it has been changed. 
	- **Improved Error/Alarm Feedback** : All Grbl error and alarm messages have been changed to providing a code. Each code is associated with a specific problem, so users will know exactly what is wrong without having to guess. Documentation and an easy to parse CSV is included in the repo.
	- **Extended-ASCII realtime commands** : All overrides and future real-time commands are defined in the extended-ASCII character space. Unfortunately not easily type-able on a keyboard, but helps prevent accidental commands from a g-code file having these characters and gives lots of space for future expansion.
	- **Message Prefixes** : Every message type from Grbl has a unique prefix to help GUIs immediately determine what the message is and parse it accordingly without having to know context. The prior interface had several instances of GUIs having to figure out the meaning of a message, which made everything more complicated than it needed to be.

- New OEM specific features, such as safety door parking, single configuration file build option, EEPROM restrictions and restoring controls, and storing product data information.
 
- New safety door parking motion as a compile-option. Grbl will retract, disable the spindle/coolant, and park near Z max. When resumed, it will perform these task in reverse order and continue the program. Highly configurable, even to add more than one parking motion. See config.h for details.

- New '$' Grbl settings for max and min spindle rpm. Allows for tweaking the PWM output to more closely match true spindle rpm. When max rpm is set to zero or less than min rpm, the PWM pin D11 will act like a simple enable on/off output.

- Updated G28 and G30 behavior from NIST to LinuxCNC g-code description. In short, if a intermediate motion is specified, only the axes specified will move to the stored coordinates, not all axes as before.

- Lots of minor bug fixes and refactoring to make the code more efficient and flexible.

- **NOTE:** Arduino Mega2560 support has been moved to an active, official Grbl-Mega [project](http://www.github.com/gnea/grbl-Mega/). All new developments here and there will be synced when it makes sense to.


```
List of Supported G-Codes in Grbl v1.1:
  - Non-Modal Commands: G4, G10L2, G10L20, G28, G30, G28.1, G30.1, G53, G92, G92.1
  - Motion Modes: G0, G1, G2, G3, G38.2, G38.3, G38.4, G38.5, G80
  - Feed Rate Modes: G93, G94
  - Unit Modes: G20, G21
  - Distance Modes: G90, G91
  - Arc IJK Distance Modes: G91.1
  - Plane Select Modes: G17, G18, G19
  - Tool Length Offset Modes: G43.1, G49
  - Cutter Compensation Modes: G40
  - Coordinate System Modes: G54, G55, G56, G57, G58, G59
  - Control Modes: G61
  - Program Flow: M0, M1, M2, M30*
  - Coolant Control: M7*, M8, M9
  - Spindle Control: M3, M4, M5
  - Valid Non-Command Words: F, I, J, K, L, N, P, R, S, T, X, Y, Z
```
