# 电控组第三次培训作业
## 丁佳睿20263009065
## 要求补充
来源：keysking
串口反馈：AA+运行/暂停+正序/反序+亮灯时长+校验和
## 选做任务
串口的DMA和硬件I2C均在文件“333333”中运用
硬件I2C实现：在cubemx中-Connectivity-I2C1-选为I2C模式，用PB6作SCL。PB7作SDA
DMA:HAL_UART_Transmit_DMA(&huart2, (uint8_t*)txBuf, strlen(txBuf))
