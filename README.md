
# mppp报文转发

## mppp简介
mppp（multilink ppp）是把多个单独的ppp链路虚拟成一条mppp链路，其主要目的是：
1. 提高链路带宽
2. 负载均衡
3. 备份
4. 利用报文分片，降低传输延迟

相关信息参考RFC1717

## mppp报文格式
### 长序列号报文格式
```
              +---------------+---------------+
 PPP Header:  | Address 0xff  | Control 0x03  |
              +---------------+---------------+
              | PID(H)  0x00  | PID(L)  0x3d  |
              +-+-+-+-+-+-+-+-+---------------+
 MP Header:   |B|E|0|0|0|0|0|0|sequence number|
              +-+-+-+-+-+-+-+-+---------------+
              |      sequence number (L)      |
              +---------------+---------------+
              |        fragment data          |
              |               .               |
              |               .               |
              |               .               |
              +---------------+---------------+
 PPP FCS:     |              FCS              |
              +---------------+---------------+
```
各字段含义如下：

字段 | 含义
--- |---
address | 固定为0xff
control | 固定为0x03
PID | ppp协议号，针对mppp报文，为0x003d
B | begin，表示该报文是一个分片报文的起始报文
E | end，表示该报文时一个分片报文的结束报文
sequence number | 序号，该字段如果是24bit，则是长序号类型，如果是12bit，则是短序号类型。

### 短序列号报文格式
```
PPP Header: | Address 0xff  | Control 0x03  |
            +---------------+---------------+
            | PID(H)  0x00  | PID(L)  0x3d  |
            +-+-+-+-+-------+---------------+
MP Header:   |B|E|0|0|    sequence number   |
            +-+-+-+-+-------+---------------+
            |    fragment data              |
            |               .               |
            |               .               |
            |               .               |
            +---------------+---------------+
PPP FCS:    |              FCS              |
            +---------------+---------------+
```

## 工程简介
该部分代码针对特定的场景实现mppp分片报文的发送，主要包括报文的封装，报文头部字段填充等，代码可以在多核cpu上运行。









