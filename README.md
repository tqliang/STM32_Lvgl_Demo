# LVGLMOVE

基于 **STM32F103C8T6 + FreeRTOS + LVGL** 的嵌入式图形界面项目，使用 ILI9341 SPI TFT LCD 作为显示设备。

---

## 项目信息

| 项目 | 说明 |
|---|---|
| **MCU** | STM32F103C8T6（ARM Cortex-M3，64KB Flash，20KB SRAM） |
| **RTOS** | FreeRTOS（CMSIS-RTOS v2 封装） |
| **GUI 库** | LVGL（LittlevGL） |
| **显示屏** | ILI9341（SPI 接口，240×320） |
| **编译器** | arm-none-eabi-gcc（GNU Tools for STM32） |
| **构建系统** | CMake + Ninja |
| **生成工具** | STM32CubeMX 6.14.1 + STM32CubeIDE / VS Code |

---

## 项目结构

```
LVGLMOVE/
├── Core/                       # 应用层核心代码
│   ├── Inc/                    # 头文件（FreeRTOSConfig.h, main.h 等）
│   └── Src/                    # 源文件（main.c, freertos.c, HAL 配置等）
├── Drivers/                    # STM32 HAL 驱动 + CMSIS
│   ├── CMSIS/
│   └── STM32F1xx_HAL_Driver/
├── Middlewares/
│   └── Third_Party/
│       └── FreeRTOS/           # FreeRTOS 内核源码
├── libs/                       # 用户库
│   ├── ili9341.c/h             # ILI9341 显示屏驱动
│   └── fonts.c/h               # 字库文件
├── cmake/                      # CMake 辅助文件
├── build/                      # 构建输出（Debug）
├── LVGLMOVE.ioc                # CubeMX 项目配置
├── STM32F103XX_FLASH.ld        # 链接脚本
└── CMakeLists.txt              # 顶层 CMake 构建文件
```

---

## 引脚连接

### SPI1（ILI9341 LCD）

| 引脚 | 功能 | 说明 |
|---|---|---|
| PA5 | SCK | SPI 时钟 |
| PA6 | MISO | SPI 主入从出 |
| PA7 | MOSI | SPI 主出从入 |
| PB0 | LCD_RST | 屏幕复位 |
| PB1 | LCD_DC | 数据/命令选择 |
| PB10 | LCD_CS | 片选信号 |

### SWD 调试接口

| 引脚 | 功能 |
|---|---|
| PA13 | SWDIO |
| PA14 | SWCLK |

---

## 快速开始

### 前置要求

- [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) 或 VS Code + STM32 扩展
- ST-Link / J-Link 调试器
- ILI9341 SPI TFT LCD 屏幕

### 编译

**方式一：VS Code（推荐）**

在 VS Code 中打开项目，确保已安装 STM32 VS Code Extension：
1. `Ctrl+Shift+P` → `CMake: Configure`
2. `Ctrl+Shift+P` → `CMake: Build`

**方式二：命令行**

```powershell
cd LVGLMOVE
cmake -B build/Debug -G Ninja -DCMAKE_BUILD_TYPE=Debug
cmake --build build/Debug
```

### 烧录

**使用 STM32CubeIDE：**
- `Run → Debug` 或 `Run → Run Configurations`

**使用 OpenOCD：**
```powershell
openocd -f interface/stlink.cfg -f target/stm32f1x.cfg -c "program build/Debug/LVGLMOVE.elf verify reset exit"
```

**使用 STM32CubeProgrammer：**
```powershell
STM32_Programmer_CLI.exe -c port=SWD -d build/Debug/LVGLMOVE.elf -v -rst
```

---

## 当前功能

- [x] FreeRTOS 任务创建与调度
- [x] ILI9341 屏幕初始化与文字显示
- [ ] LVGL 图形界面（已集成，待启用）
- [ ] 触屏支持
- [ ] LVGL 动画与交互

---

## FreeRTOS 配置

| 任务 | 优先级 | 栈大小 | 入口函数 |
|---|---|---|---|
| defaultTask | Normal | 128 words (512 bytes) | StartDefaultTask() |

---

## 依赖版本

| 组件 | 版本 |
|---|---|
| STM32Cube FW_F1 | v1.8.7 |
| FreeRTOS | CMSIS-RTOS v2 |
| LVGL | v9.x |
| arm-none-eabi-gcc | 14.3.1 |

---

## License

本项目基于 STM32CubeMX 生成，HAL 库和 FreeRTOS 部分遵循各自的许可协议。