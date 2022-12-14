## 简介

- Modbus是一种串行通信协议，分为RTU、ASCII、TCP
- Modbus是基于现有OSI 网络模型上的应用协议
- Modbus协议可以解决工厂不同种类设备的数据采集问题
- ModBus协议最基本的通信单位是帧，整个ModBus帧又被称之为应用数据单元（ADU），ADU中又包含了协议数据单元（PDU）用于传真正需要传输的数据

![image-20221108103935632](C:\Users\BDA\Documents\Note\pic\image-20221108103935632.png)

- Modbus协议是一个主/从架构的协议。在同一个Modbus网络中同一时刻只有一个节点是主（master）节点，其他使用Modbus协议参与通信的节点是从（slave）节点，从节点的最大编号为247。每一个slave设备都有一个唯一的地址，子节点在没有收到主节点请求时不会发送数据。各个子节点之间不会直接相互通信
- 在同一个时刻，主节点只会发起一个Modbus事务处理，主要包含两种形式
  - 单播模式：一个请求一个响应
  - 广播模式：多个请求，可以不用响应

## Modbus RTU

### 存储区

- 输入/输出   线圈/寄存器
- 线圈1位，一般存储布尔值，寄存器2字节16位，一般存储数据
- 范围
  - 5位：Y   XXXX   标准地址
    - 0   0001 - 0   9999
    - 1   0001 - 1   9999
  - 6位：Y   XXXXX   扩展地址
    - 2  00001 - 2   65536
    - 3  00001 - 3   65536

### 通信（读写）

- 读输出线圈	01
- 读输入线圈	02
- 读输出寄存器	03
- 读输入寄存器	04
- 写单个输出线圈	05
- 写单个输出寄存器	06
- 写多个输出线圈	15
- 写多个输出寄存器	16

### 协议

- 报文格式：从站地址（设备编号）+ 功能码 + 数据 + 校验 

  - 1byte + 1byte + N byte + 2byte

- 写入在数据方面有更多的操作

- 读取时

  ```shell
  #请求
  01 站地址
  03 读输出寄存器
  00 00 起始寄存器地址
  00 02 寄存器读取长度
  C4 0B CRC校验
  #响应
  01 站地址
  03 读输出寄存器
  04 读取的字节数
  01 46  01 38 具体读取的字节
  5A 59 CRC校验
  ```

- 写入时

  ```shell
  ```

  