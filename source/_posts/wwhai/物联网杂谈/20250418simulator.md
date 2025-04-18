---
title: Arduino-Modbus 测试小工具
date:  2025-04-18 20:49:29
index_img:  /uploads/2025/SBC-NodeMCU-1.png
tags:
- 物联网杂谈

categories:
- 物联网杂谈

author: wangwenhai
---
本文作者：[wangwenhai] # 概要：一个非常简单的Arduino-Modbus协议模拟器
<!-- more -->

# Arduino Meterbus 仪表模拟器

## 项目简介
本项目是一个基于ESP8266的Modbus从站模拟器，用于模拟Meterbus仪表的行为。它能够响应Modbus主站的请求，并返回预定义的数据。
![NodeMCU](/uploads/2025/SBC-NodeMCU-1.png)

<table>
        <tr>
            <td><img src="/uploads/2025/8266-1.png" alt="Image 1"></td>
            <td><img src="/uploads/2025/8266-2.png" alt="Image 2"></td>
        </tr>
</table>

## 功能描述
- 支持Modbus功能码：
  - 0x01: 读取线圈状态
  - 0x02: 读取离散输入
  - 0x03: 读取保持寄存器
  - 0x04: 读取输入寄存器
- 内置CRC16校验功能
- 通过串口与Modbus主站通信

## 使用方法
1. 使用Arduino IDE打开项目
2. 选择正确的开发板（ESP8266）和端口
3. 上传代码到开发板
4. 使用Modbus主站设备或软件通过串口与模拟器通信

## 硬件要求
- ESP8266开发板
- USB转串口适配器

## 软件依赖
- Arduino IDE
- ESP8266开发板支持包

## 项目结构
```
.
├── src
│   └── modbus_slave.ino
├── platformio.ini
└── README.md
```
## 代码

**modbus_slave.ino**

```cpp
#include <Arduino.h>
const int BUFFER_SIZE = 64;
const byte READ_HOLDING_REGISTERS = 0x03;

byte receiveBuffer[BUFFER_SIZE];
int receiveIndex = 0;

void setup()
{
  Serial.begin(9600);
}

unsigned inline int crc16(byte *data, int length)
{
  unsigned int crc = 0xFFFF;
  for (int i = 0; i < length; i++)
  {
    crc ^= (unsigned int)data[i];
    for (int j = 0; j < 8; j++)
    {
      if (crc & 0x0001)
      {
        crc >>= 1;
        crc ^= 0xA001;
      }
      else
      {
        crc >>= 1;
      }
    }
  }
  return crc;
}

void sendReadCoilsResponse(uint8_t slaveId, uint16_t coilCount)
{
  uint8_t byteCount = (coilCount + 7) / 8;
  uint8_t responseBuffer[3 + byteCount];

  responseBuffer[0] = slaveId;
  responseBuffer[1] = 0x01;
  responseBuffer[2] = byteCount;

  for (uint8_t i = 0; i < byteCount; i++)
  {
    responseBuffer[3 + i] = 0xFF;
  }

  uint16_t crc = crc16(responseBuffer, 3 + byteCount);
  uint8_t crcLow = crc & 0xFF;
  uint8_t crcHigh = (crc >> 8) & 0xFF;
  for (uint8_t i = 0; i < (3 + byteCount); i++)
  {
    Serial.write(responseBuffer[i]);
  }

  Serial.write(crcLow);
  Serial.write(crcHigh);
}

void sendDynamicBytes(byte slaveId, unsigned short quantity)
{
  byte byteCount = quantity * 2;
  byte responseBuffer[3 + byteCount];

  responseBuffer[0] = slaveId;
  responseBuffer[1] = READ_HOLDING_REGISTERS;
  responseBuffer[2] = byteCount;

  for (int i = 0; i < byteCount; i++)
  {
    responseBuffer[3 + i] = 0xAB;
    responseBuffer[4 + i] = 0xCD;
  }

  unsigned int crc = crc16(responseBuffer, 3 + byteCount);
  byte crcLow = crc & 0xFF;
  byte crcHigh = (crc >> 8) & 0xFF;

  for (int i = 0; i < (3 + byteCount); i++)
  {
    Serial.write(responseBuffer[i]);
  }
  Serial.write(crcLow);
  Serial.write(crcHigh);
}

void processModbusRequest()
{
  byte slaveId = receiveBuffer[0];
  byte functionCode = receiveBuffer[1];
  // byte startAddressHigh = receiveBuffer[2];
  // byte startAddressLow = receiveBuffer[3];
  byte quantityHigh = receiveBuffer[4];
  byte quantityLow = receiveBuffer[5];
  // byte crcLow = receiveBuffer[6];
  // byte crcHigh = receiveBuffer[7];
  unsigned short quantity = (quantityHigh << 8) | quantityLow;
  switch (functionCode)
  {
  case 1: // Read Coils
    sendReadCoilsResponse(slaveId, quantity);
    break;
  case 2: // Read Discrete Inputs
    sendReadCoilsResponse(slaveId, quantity);
    break;
  case 3: // Read Holding Registers
  case 4: // Read Input Registers
    sendDynamicBytes(slaveId, quantity);
    break;
  default:
    break;
  }
}

void loop()
{
  while (Serial.available() > 0)
  {
    byte incomingByte = Serial.read();
    receiveBuffer[receiveIndex++] = incomingByte;
    if (receiveIndex >= 8)
    {
      processModbusRequest();
      receiveIndex = 0;
    }
  }
  delay(30);
}
```
## 许可证
MIT License