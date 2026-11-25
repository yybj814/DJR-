# 电控组第三次培训作业

姓名：丁佳睿　学号：20263009065

## 一、参考资料与驱动来源
- 串口数据包案例参考：keysking 串口通信视频讲解，使用keysking提供的串口助手
- OLED 驱动使用队里资料包提供的版本（软件 IIC）
- 硬件 IIC 与 DMA 为在资料基础上自行扩展

## 二、串口反馈格式（自己定义）
单片机校验通过并应用设置后，通过串口回传英文状态，格式：

    LED:RED\r\n

颜色为 RED / YELLOW / GREEN 之一，表示当前点亮的灯。
基础要求使用 HAL_UART_Transmit_IT() 中断方式发送（在文件"RUANJIANI2CIT"中展现）。

## 三、串口控制数据包

    AA  RUN  DIR  STEP_L  STEP_H  CHECK

- 帧头 AA；RUN 00=暂停 01=运行；DIR 00=正向 01=反向
- STEP 为切换间隔，低字节在前，范围 100~2000ms
- CHECK = (AA+RUN+DIR+STEP_L+STEP_H) % 256
- 使用串口接收中断逐字节接收，自定义 UART_Packet 结构体解析，
  帧头或校验错误时丢弃不处理

## 四、选做说明

### 4.1 硬件 IIC（在 "YINGJIANI2CDMA" 工程中）
CubeMX → Connectivity → I2C1 设为 I2C 模式，PB6=SCL、PB7=SDA。
关键源码：

    void OLED_Send(uint8_t *data, uint8_t len) {
      HAL_I2C_Master_Transmit(&hi2c1, OLED_ADDRESS, data, len, 100);
    }

### 4.2 DMA 发送（在 "YINGJIANI2CDMA" 工程中）
用 DMA 替代中断发送反馈,更省 CPU。
关键源码：

    static char txBuf[32];
    strcpy(txBuf, "LED:");
    strcat(txBuf, led);
    strcat(txBuf, "\r\n");
    HAL_UART_Transmit_DMA(&huart2, (uint8_t*)txBuf, strlen(txBuf));


