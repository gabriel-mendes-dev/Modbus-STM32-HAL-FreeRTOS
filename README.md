# Modbus library for STM32 Microcontrollers
USART and USB-CDC Modbus RTU Master and Slave library for STM32 microcontrollers 
based on Cube HAL and FreeRTOS.



Includes multiple examples for popular development boards including BluePill, NUCLEO-64, 
NUCLEO-144 and Discovery Boards (Cortex-M3/M4/M7).

This is a port of the Modbus library for Arduino: https://github.com/smarmengol/Modbus-Master-Slave-for-Arduino

Video demo for STM32F4-dicovery board and TouchGFX: https://youtu.be/XDCQvu0LirY

## Characteristics:
- Portable to any STM32 MCU supported by ST Cube HAL.
- Multithread-safe implementation based on FreeRTOS. 
- Multiple instances of Modbus (Master and/or Slave) can run concurrently in the same MCU,
  only limited by the number of available UART/USART of the MCU.
- RS232 and RS485 compatible.
- NEW: USB-CDC support 



## File structure
```
├── LICENSE
├── README.md
├── ModbusBluepill --> STM32F103C8 USART Slave example
├── ModbusBluepillUSB --> STM32F103C8 USART + USB-CDC Master and Slave example
├── ModbusF103 --> NUCLEO64-F103RB Modbus Master and Slave example
├── ModbusF429 --> NUCLEO144-F429ZI Modbus Slave example
├── ModbusH743 --> NUCLEO144-H743ZI Modbus Slave example
├── ModbusF303 --> NUCLEO64-F303RE Modbus Slave example
├── ModbusSTM32F4-discovery --> STM32F4-discovery TouchGFX + Modbus Master example
├── MODBUS-LIB --> Library Folder
    ├── Inc
    │   └── Modbus.h 
    └── Src
        ├── Modbus.c 
        └── UARTCallback.c 
```
## How to use the examples
Examples provided for STM32CubeIDE Version: 1.3.0 https://www.st.com/en/development-tools/stm32cubeide.html.

- Import the examples in the STM32Cube IDE from the system folder
- Connect your NUCLEO board
- Compile and start your debugging session!
- If you need to adjust the Baud rate or any other parameter use the Cube-MX assistant (recommended). If you change the USART port you need to enable the interrupts for the selected USART. Check UARTCallback.c for more details.
- For the ModbusF103 example, you need and extra USB-to-serial adapter, or you can connect the Master and Slave instances in a loopback (USART1 <--> USART3).

### NOTE:
The USB-CDC example supports only the Bluepill development board. It has not been validated with other development boards.
To use this example, you need to activate the USB-CDC in the following line in Modbus.h file:

https://github.com/alejoseb/Modbus-STM32-HAL-FreeRTOS/blob/1d98194b3f0643edad694bf85cef9b25189204a5/MODBUS-LIB/Inc/Modbus.h#L21


Modbus instances over USART and USB-CDC  can run simultaneously in Master or Slave modes concurrently. It is restricted to 1 USB-CDC interface and 3 USART interfaces for the Bluepill Board.


## How to port to your own MCU
- Create a new project in STM32Cube IDE
- Configure a USART and activate the global interrupt of it
- Configure the `Preemption priority` of USART interrupt to a lower priority (5 or a higher number for a standard configuration) than your FreeRTOS scheduler. This parameter is changed in the NVIC configuration pane.
- Import the Modbus library folder (MODBUS-LIB) using drag-and-drop from your host operating system to your STM32Cube IDE project
- When asked, choose link folders and files
- Update the include paths in the project's properties to include the `Inc` folder of MODBUS-LIB folder
- Instantiate a new global modbusHandler_t and follow the examples provided in the repository 
- `Note:` If the USART interrupts service for other purposes you have to modify the UARTCalback.c file accordingly


## Recommended Modbus Master and Slave testing tools for Linux and Windows

### Master client Qmodbus
Linux:    https://launchpad.net/~js-reynaud/+archive/ubuntu/qmodbus

Windows:  https://sourceforge.net/projects/qmodbus/

### Slave simulator
Linux: https://sourceforge.net/projects/pymodslave/

Windows: https://sourceforge.net/projects/modrssim2/

## TODOs:
- Implement wrapper functions for Master function codes. Currently, telegrams are defined manually. 
- Improve function documentation
- ~~Improve the queue for data reception; the current method is too heavy it should be replaced with a simple buffer, a stream, or another FreeRTOS primitive.~~ Solved Queue repalced by a Ring Buffer (03/19/2021)
- ~~Test with Rs485 transceivers (implemented but not tested)~~ Verified with MAX485 transceivers (01/03/2021)
- MODBUS TCP implementation
* * *
# Modbus函式庫For STM32微處理器
* 適用於STM32(Cortex-M3/M4/M7)
* 該函式庫可以實作 Modbus Rtu [Master模式] 與 [Slave模式] 支援USART 與 USB-CDC
* 使用Stm32cube HAL庫 與 Freertos時實系統
* 也可導入該函式庫到BluePill, NUCLEO-64, NUCLEO-144板子
* Arduino演示影片 : https://github.com/smarmengol/Modbus-Master-Slave-for-Arduino
* STM32F4-dicovery 板子 與 TouchGFX 演示 : https://youtu.be/XDCQvu0LirY
## 特點
* 可移植到ST Cube HAL支持的任何STM32 MCU。
* 基於FreeRTOS的多線程安全實現。 
* Modbus的多個實例（主站和/或從站）可以在同一MCU中同時運行，僅受MCU可用UART / USART數量的限制(板子有多少串口就可以用多少)。 
* 支援RS232與RS485(注意! 485需要接RE DE發送/接收 控制腳位)
* 新! USB-CDC 支援

## 目錄
```
├── LICENSE
├── README.md
├── ModbusBluepill --> STM32F103C8 USART Slave example
├── ModbusBluepillUSB --> STM32F103C8 USART + USB-CDC Master and Slave example
├── ModbusF103 --> NUCLEO64-F103RB Modbus Master and Slave example
├── ModbusF429 --> NUCLEO144-F429ZI Modbus Slave example
├── ModbusH743 --> NUCLEO144-H743ZI Modbus Slave example
├── ModbusF303 --> NUCLEO64-F303RE Modbus Slave example
├── ModbusSTM32F4-discovery --> STM32F4-discovery TouchGFX + Modbus Master example
├── MODBUS-LIB --> Library Folder
    ├── Inc
    │   └── Modbus.h 
    └── Src
        ├── Modbus.c 
        └── UARTCallback.c 
```

## 如何使用範例
### 首先先去意法官方下載STM32CubeIDE :  https://www.st.com/en/development-tools/stm32cubeide.html
* 導入範例到Stm32 IDE裡
* 連接你的開發板
* 編譯你的程式，或是Debug模式(F11)
* 如果有需要修改胞率，可以使用STM32 CubeMX修改(Stm32CubeIDE已經把MX功能融合了，所以也可以用IDE修改，副檔名為ioc的就是STM32的配置檔)，有關更多信息，可以嘗試配置UARTCallback.c
* 在本範例中STM32F103 [串口1是Master] [串口3是Slave] 可以實現閉迴路測試  (USART1 <-> USART3)

### 筆記
* USB-CDC示例僅支持Bluepill開發板。 尚未與其他開發板進行驗證。 要使用此示例，您需要在Modbus.h文件的以下行中激活USB-CDC：
* https://github.com/alejoseb/Modbus-STM32-HAL-FreeRTOS/blob/1d98194b3f0643edad694bf85cef9b25189204a5/MODBUS-LIB/Inc/Modbus.h#L21
* 通過USART和USB-CDC的Modbus實例可以同時在主模式或從模式下運行。 Bluepill板僅限於1個USB-CDC接口和3個USART接口。 
* * *
## 如何把函式庫導入到自己的專案
* 首先STM32CUBEIDE 開啟新專案
* 配置串口的中斷(NVIC)!!
* 配置FreeRTOS CMSIS V2.0 設定為開啟，並且設定中斷優先級(5或更高的數字[{預設/標準}配置])
* 在FreeRTOS裡新增Task任務
* Slave名子 myTaskSlave 函式名稱 StartSlaveTask
* Master名子 myTaskMaster 函式名稱 StartMasterTask
* 導入頭文件，打開stm32目錄資料夾，對專案右鍵，最下面的配置，找到include導入 MODBUS-LIB
* Main.c 初始化函式庫
* Main.h 新增全局函數 (extra)
* 提示: 如果USART中斷服務用於其他目的，則必須相應地修改UARTCalback.c文件
* * *
# 推薦的適用於Linux和Windows的Modbus主從測試工具 
### Master client Qmodbus
Linux:    https://launchpad.net/~js-reynaud/+archive/ubuntu/qmodbus

Windows:  https://sourceforge.net/projects/qmodbus/

### Slave simulator
Linux: https://sourceforge.net/projects/pymodslave/

Windows: https://sourceforge.net/projects/modrssim2/
* * *
# 目標
* 為主功能代碼實現包裝器功能。 當前，telegrams是手動定義的。 
* 改進功能文檔 
* ~~改善數據接收隊列； 當前方法過於繁重，應將其替換為簡單的緩衝區，流或其他FreeRTOS原語。 已解決的隊列由環形緩衝區重新填充 [Ring Wiki](https://zh.wikipedia.org/wiki/%E7%92%B0%E5%BD%A2%E7%B7%A9%E8%A1%9D%E5%8D%80)（03/19/2021） ~~
* ~~使用Rs485收發器進行測試（已實現，但未經測試）已通過MAX485收發器進行了驗證（01/03/2021） ~~
* Modbus TCP功能實現
* * * 
