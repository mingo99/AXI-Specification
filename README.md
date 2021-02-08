**AXI Specification**
-

- [1. Introduction](#1-introduction)
  - [1.1. About the AXI Protocol](#11-about-the-axi-protocol)
    - [1.1.1. Features](#111-features)
    - [1.1.2. Key features](#112-key-features)
    - [1.1.3. Includes](#113-includes)
  - [1.2. AXI Architecture](#12-axi-architecture)
    - [1.2.1. Description](#121-description)
    - [1.2.2. Channel Definition](#122-channel-definition)
    - [1.2.3. Interface&Interconnect](#123-interfaceinterconnect)
    - [1.2.4. Register Slices](#124-register-slices)
  - [1.3. Terminology](#13-terminology)
    - [1.3.1. Components and Topology](#131-components-and-topology)
    - [1.3.2. Transactions](#132-transactions)
    - [1.3.3. Caches and Cache Operation](#133-caches-and-cache-operation)
    - [1.3.4. Temporal description](#134-temporal-description)
- [2. Signal Description](#2-signal-description)
  - [2.1. Global signals](#21-global-signals)
  - [2.2. Write address channel signals](#22-write-address-channel-signals)
    - [2.2.1. From Master](#221-from-master)
    - [2.2.2. From Slave](#222-from-slave)
  - [2.3. Write data channel signals](#23-write-data-channel-signals)
    - [2.3.1. From Master](#231-from-master)
    - [2.3.2. From Slave](#232-from-slave)
  - [2.4. Write response channel signals](#24-write-response-channel-signals)
    - [2.4.1. From Slave](#241-from-slave)
    - [2.4.2. From Master](#242-from-master)
  - [2.5. Read address channel signals](#25-read-address-channel-signals)
    - [2.5.1. From Master](#251-from-master)
    - [2.5.2. From Slave](#252-from-slave)
  - [2.6. Read data channel signals](#26-read-data-channel-signals)
    - [2.6.1. From Slave](#261-from-slave)
    - [2.6.2. From Master](#262-from-master)
  - [2.7. Low-power interface signals](#27-low-power-interface-signals)
- [3. Single Interface Requirements](#3-single-interface-requirements)
  - [3.1. Clock and reset](#31-clock-and-reset)
  - [3.2. Basic read and write transactions](#32-basic-read-and-write-transactions)
    - [3.2.1. Handshake process](#321-handshake-process)
    - [3.2.2. Channel signaling requirements](#322-channel-signaling-requirements)
  - [3.3. Relationships between the channels](#33-relationships-between-the-channels)
    - [3.3.1. Relationships](#331-relationships)
    - [3.3.2. Dependenci between channel handshake signals](#332-dependenci-between-channel-handshake-signals)
    - [3.3.3. Legacy considerations](#333-legacy-considerations)
  - [3.4. Transaction structure](#34-transaction-structure)
    - [3.4.1. Address structure](#341-address-structure)
    - [3.4.2. Pseudocode description of the transfers](#342-pseudocode-description-of-the-transfers)
    - [3.4.3. Data read and write structure](#343-data-read-and-write-structure)
    - [3.4.4. Read and write response structure](#344-read-and-write-response-structure)
- [4. Transaction Attributes](#4-transaction-attributes)
  - [4.1. Transaction types and attributes](#41-transaction-types-and-attributes)
  - [4.2. AXI3 memory attribute signaling](#42-axi3-memory-attribute-signaling)
    - [4.2.1. Transaction attribute encodeing](#421-transaction-attribute-encodeing)
  - [4.3. AXI4 changes to memory attribute signaling](#43-axi4-changes-to-memory-attribute-signaling)
    - [4.3.1. *AxCACHE[1], Modifiable*](#431-axcache1-modifiable)
    - [4.3.2. Updated meaning of Read-Allocate and Write-Allocate](#432-updated-meaning-of-read-allocate-and-write-allocate)
  - [4.4. Memory types](#44-memory-types)
    - [4.4.1. Memory type encoding](#441-memory-type-encoding)
    - [4.4.2. Memory type requirements](#442-memory-type-requirements)
  - [4.5. Mismatched memory attributes](#45-mismatched-memory-attributes)
    - [4.5.1. Mismatched memory attributes](#451-mismatched-memory-attributes)
    - [4.5.2. Changing memory attributes](#452-changing-memory-attributes)
  - [4.6. Transaction buffering](#46-transaction-buffering)
    - [4.6.1. Write transactions](#461-write-transactions)
    - [4.6.2. Read transactions](#462-read-transactions)
  - [4.7. Access permissions](#47-access-permissions)
    - [4.7.1. Signals](#471-signals)
    - [4.7.2. Protection attributes](#472-protection-attributes)
- [5. Transaction Identifiers](#5-transaction-identifiers)
  - [5.1. AXI transaction identifiers](#51-axi-transaction-identifiers)
  - [5.2. ID signals](#52-id-signals)
    - [5.2.1. Channel transaction ID](#521-channel-transaction-id)
    - [5.2.2. Read data ordering](#522-read-data-ordering)
    - [5.2.3. Write data ordering](#523-write-data-ordering)
    - [5.2.4. Interconnect use of transaction identifiers](#524-interconnect-use-of-transaction-identifiers)
- [6. AXI Ordering Model](#6-axi-ordering-model)
  - [6.1. AXI ordering model overview](#61-axi-ordering-model-overview)
  - [6.2. Memory locations and Peripheral regions](#62-memory-locations-and-peripheral-regions)
  - [6.3. Transactions and ordering](#63-transactions-and-ordering)
  - [6.4. Observation and completion definitions](#64-observation-and-completion-definitions)
    - [6.4.1. Observation](#641-observation)
    - [6.4.2. Completion](#642-completion)
  - [6.5. Master ordering guarantees](#65-master-ordering-guarantees)
    - [6.5.1. Guarantees before a completion response is received](#651-guarantees-before-a-completion-response-is-received)
    - [6.5.2. Guarantees from a completion response](#652-guarantees-from-a-completion-response)
    - [6.5.3. Response ordering guarantees](#653-response-ordering-guarantees)
  - [6.6. Ordering requirements](#66-ordering-requirements)
    - [6.6.1. Slave ordering requirments](#661-slave-ordering-requirments)
    - [6.6.2. Interconnect ordering requirments](#662-interconnect-ordering-requirments)
  - [6.7. Response before the endpoint](#67-response-before-the-endpoint)
    - [6.7.1. Early read response](#671-early-read-response)
    - [6.7.2. Early write response](#672-early-write-response)
  - [6.8. Ordered write observation](#68-ordered-write-observation)
    - [6.8.1. Ordered write observation](#681-ordered-write-observation)
    - [6.8.2. Ordered_Write_Observation](#682-ordered_write_observation)
- [7. Atomic Accesses](#7-atomic-accesses)
  - [7.1. Single-copy atomicity size](#71-single-copy-atomicity-size)
    - [7.1.1. Single-copy atomicity size](#711-single-copy-atomicity-size)
    - [7.1.2. Multi-copy write atomicity](#712-multi-copy-write-atomicity)
  - [7.2. Exclusive accesses](#72-exclusive-accesses)
  - [7.3. Locked accesses](#73-locked-accesses)
  - [7.4. Atomic access signaling](#74-atomic-access-signaling)

# 1. Introduction

## 1.1. About the AXI Protocol 

### 1.1.1. Features

- 高性能、低延迟
- 高频通信、结构简单
- 满足各种组件的接口要求
- 适用于具有较高初始访问延迟的内存控制器
- 互联架构实施高度灵活
- 兼容AHB&APB接口

### 1.1.2. Key features

- 地址/控制信息和数据单独传输
- 支持非对齐数据传输，使用写选通信号（按字节）
- 使用突发为基础的事务传输，主机仅需发出首地址
- 独立的读写数据通道，提供低开销的直接数据访问
- 支持outstanding操作，即Master不用等待上一个事务传输完成就可发送下一个事务的地址
- 支持out of order操作，即Slave返回数据时可不必按照Master发送的地址顺序
- 允许添加寄存器级以提供时序收敛

### 1.1.3. Includes

- AXI4 Lite
- AXI5 Lite
  
## 1.2. AXI Architecture

### 1.2.1. Description

- **Transaction channels**
  - Write address，写地址通道，信号以AW开头
  - Write data，写数据通道，信号以W开头
  - Write response，写回应通道，信号以B开头
  - Read address，读地址通道，信号以AR开头
  - Read data，读数据通道，信号以R开头

- **Channel architecture of transaction**
  - Channel architecture of writes ![写事务的通道架构](README.assets/1.jpg)
  - Channel architecture of reads ![读事务的通道架构](README.assets/2.jpg)


### 1.2.2. Channel Definition

五个独立的通道都采用VALID和READY的握手协议，此外读写数据通道还包括一个LAST信号
- **Read&Write Address Channels**  
    读写事务拥有自己的地址通道，地址通道可以携带所需的地址和控制信息
- **Read Data Channel**  
    包括数据总线（可支持8－1024位）和一个读响应信号
- **Write Data Channel**  
    1、包括数据总线（可支持8－1024位）和一个字节大小的通道选通信号（每八个数据位一个字节通道选通信号，指示数据总线的哪个字节有效）    
    2、写数据通道信息始终被视为已缓冲，因此主机可以执行写事务，而无需从机确认先前的写事务
- **Write Data Channel**  
    1、主要支持从机返回响应信号，表示传输完成
    2、每个burst传输仅需一个完成信号

### 1.2.3. Interface&Interconnect

- **Interface**  
    主机和Interconnect、从机和Interconnect以及主从机之间均需要接口

- **Interconnect Approaches**
    - **Shared address and data buses**   
        地址和数据总线均共享
    - **Shared address buses and multiple data buses**  
        地址总线共享、数据总线私有
    - **Multilayer, with mutiple address and data buses**  
        多层结构，数据和地址总线均私有

### 1.2.4. Register Slices

- 寄存器切片对AXI总线的关键路径进行切断和分割，以获得更好的时序性能

## 1.3. Terminology

### 1.3.1. Components and Topology

- Component
- Master component
- Slave component
    - Memory slave component
    - Peripheral slave component
- Interconnect component

### 1.3.2. Transactions

- Transaction
- Burst
- Beats

### 1.3.3. Caches and Cache Operation

- Cache
- Cache line

### 1.3.4. Temporal description

- The AXI specification uses the term in a timely manner.

# 2. Signal Description

## 2.1. Global signals

- **ACLK**
    Clock source

- **ARESETn**  
    Reset source   
    Active Low

## 2.2. Write address channel signals

### 2.2.1. From Master

- **AWID**  
    Write address ID，写地址ID，用来标志一组写信号
- **AWADDR**   
    Write address，写地址，突发传输首地址
- **AWLEN**   
    Burst length，突发传输的传输次数
- **AWSIZE**   
    Burst size，每次传输的字节数
- **AWBURST**   
    Burst type，加上size信息用来计算突发传输内每次传输的地址
- **AWLOCK**   
    Lock type，总线锁信号，可提供操作的原子性
- **AWCACHE**   
    Cache type，内存类型，表明一次传输是怎样通过系统的
- **AWPROT**  
    Protection type，指示事务的保护级别（正常、特权、安全）以及事务是数据访问还是指令访问
- **AWQOS**   
    写事务服务质量
- **AWREDION**   
    区域标志，能实现单一物理接口对应的多个逻辑接口
- **AWUSER**   
    用户自定义信号
- **AWVALID**   
    1、Write address valid，指示写地址总线和控制信息是否有效，高电平有效  
    2、在AWREADY变高前一直保持

### 2.2.2. From Slave

- **AWREADY**   
    Write address ready，指示从机可以接收地址和控制信息，高电平有效

## 2.3. Write data channel signals

### 2.3.1. From Master

- **WID**  
    Write ID tag，写过程ID，一次写传输的ID，必须和AWID相匹配
- **WDATA**  
    Write data，可支持8-1024位数据传输
- **WSTRB**  
    Write strobes（写选通信号），指示当前传输的有效数据段,每一位对应一个字节
- **WLAST**  
    Write last，表明突发传输的最后一次传输
- **WUSER**  
    用户自定义信号
- **WVALID**  
    Write valid，指示写数据和写跳变信息有效与否，高有效

### 2.3.2. From Slave

- **WREADY**  
    Write ready，高电平时才可进行写数据

## 2.4. Write response channel signals

### 2.4.1. From Slave

- **BID**  
    Response ID，写响应ID标签，和AWID相对应
- **BRESP**  
    1、Write response，表示写事务的状态       
    2、包括4种状态：OKAY、EXOKAY、SLVERR、DECERR
- **BUSER**  
    用户自定义信号
- **BVALID**  
    Write response valid，高电平时写响应信号有效

### 2.4.2. From Master

- **BREADY**    
    Response ready，高电平时表示主机可接受响应信号

## 2.5. Read address channel signals

### 2.5.1. From Master
- **ARID**  
    Write address ID，用来标志一组读信号
- **ARADDR**  
    Write address，给出一次读突发传输的读地址
- **ARLEN**  
    Burst length，给出突发传输的次数
- **ARSIZE**  
    Burst size，给出每次传输的字节数
- **ARBURST**  
    Burst type
- **ARLOCK**  
    Lock type，总线锁信号，可提供操作的原子性
- **ARCACHE**  
    Cache type，内存类型，表明一次传输是怎样通过系统的
- **ARPROT**  
    Protection type，表明一次传输的特权级及安全等级
- **ARQOS**  
    服务质量
- **ARREGION**  
    区域标志，能实现单一物理接口对应的多个逻辑接口
- **ARUSER**  
    用户自定义信号
- **ARVALID**  
    有效信号，表明此通道的地址控制信号有效

### 2.5.2. From Slave

- **ARREADY**  
    表明Slave可以接收地址和对应的控制信号

## 2.6. Read data channel signals

### 2.6.1. From Slave

- **RID**  
    Read ID tag，一次读传输的ID
- **RDATA**  
    Read data，可支持8-1024位数据传输
- **RRESP**  
    Read response，表明读传输的状态
- **RLAST**  
    Indicating last data transfer in a read transaction
- **RUSER**  
    用户自定义信号
- **RVALID**  
    Read valid，表明此通道信号有效

### 2.6.2. From Master

- **RREADY**  
    Read ready，表明主机能够接收读数据和响应信息

## 2.7. Low-power interface signals

- **CSYSREQ**  
    1、System low-power request  
    ​2、Clock controller --> Peripheral device
- **CSYSACK**  
    1、Low-power request acknowlegement  
    2、Peripheral device --> Clock controller
- **CACTIVE**  
    1、Clock active   
    2、Peripheral device --> Clock controller

# 3. Single Interface Requirements

## 3.1. Clock and reset

- **Clock**
    - 每个AXI接口都有一个单独的时钟信号 *ACLK* 
    - 所有输入信号均在 *ACLK* 的上升沿进行采样 
    - 所有输出信号必须在 *ACLK* 上升沿之后改变 
- **Reset**
    - **特点**  
        - 低电平有效，异步复位、同步释放
    - **复位期间对个接口信号要求**  
        - 主接口信号 *ARVALID*、*AWVALID* 和 *WVALID* 必须拉低  
        - 从接口信号 *RVALID* 和 *BVALID* 必须拉低

## 3.2. Basic read and write transactions

### 3.2.1. Handshake process

- **特点**  
    - 每个通道均采用以VALID/READY为握手信号的流控机制  
    - Master和Slave都可以控制数据的传输速率  
    - VALID信号由源设备产生，READY信号由目的设备产生  
    - 当VALID和READY均为高时数据才可传输

- **示例**    
    - VALID before READY handshake ![VbR](README.assets/3.jpg)
    - READY before VALID handshake ![RbV](README.assets/4.jpg)
    - VALID with READY handshake   ![VwR](README.assets/5.jpg)   

### 3.2.2. Channel signaling requirements

- **Write address channel**  
    - 仅当地址和控制信息有效时主机才可拉高 *AWVALID*，并一直保持到从机拉高 *AWREADY*  
    - *AWREADY* 缺省值可高可低，但一般为高，否则至少需要两个时钟周期才能完成数据传输

- **Write data channel**  
    - 仅当地址和控制信息有效时主机才可拉高 *WVALID* ，并一直保持到从机 *WREADY*   
    - 仅当从机在一个周期内可接受写数据时，*WREADY* 可缺省为高  
    - 最后一次写传输时主机需要把 *WLAS* T拉高  
    - *WVALID* 为低时，写选通信号可为任意值，但一般为低或保持之前的值  
    - 无效字节一般保持为0

- **Write response channel**  
    - 仅当写响应信号有效时从机才可拉高 *BVALID*，并一直保持到主机拉高 *BREADY*  
    - 仅当主机在一个周期内可接受写响应信号时，*BREADY* 可缺省为高

- **Read address channel**  
    - 仅当地址和控制信息有效时主机才可拉高 *ARVALID*，并一直保持到从机拉高 *ARREADY*  
    - *ARREADY* 缺省值可高可低，但一般为高，否则至少需要两个时钟周期才能完成数据传输  

- **Read data channel**  
    - 仅当地址和控制信息有效时主机才可拉高 *RVALID*，并一直保持到从*RREADY*  
    - 仅当从机在一个周期内可接受写数据时，*RREADY* 可缺省为高  
    - 最后一次写传输时主机需要把 *RLAST* 拉高  
    - 无效字节一般保持为0 

## 3.3. Relationships between the channels

### 3.3.1. Relationships

- 写响应必须在写事务中的最后一次传输后
- 读数据跟随读地址
- 通道间的握手信号存在依赖关系
- Note
    - Master发出读写请求之后，必须能够提供或接收相关数据，并且在事务传输期间不依赖于来自该Master的其他事务
    - Master可以依赖从使用相同ID的事务按顺序返回的读取数据，因此，Master仅需要足够的存储空间来存储来自具有不同ID的事务的读取数据。
  
### 3.3.2. Dependenci between channel handshake signals

- **In Any Transanction**  
    - 不同AXI组件之间的VALID和READY信号没有依赖关系  
    - READY信号可以等待VALID信号置位  
- **In Read Transanction**  
    ![读通道握手信号依赖关系](README.assets/6.jpg)  
    - 从机在拉高ARREADY之前可以等待ARVALID  
    - 从机在拉高RVALID之前必须等待ARVALID和ARREADY
- **In Write Transanction**
    - AXI3 
        ![AXI3写通道握手信号依赖关系](README.assets/7.jpg)  
        - 主机在拉高AWVALID或WVALID之前不必等待AWREADY或WREADY
            WVALID不必等待AWREADY可以防止在从机拉高AWREADY之前等待WVALID的情况下造成死锁    
        - 从机可以在拉高AWREADY或WREADY之前等待AWVALID或WVALID  
        - 从机在拉高BVALID之前必须等待WVALID和WREADY以及WLAST
    - AXI4&AXI5     
        ![AXI4写通道握手信号依赖关系](README.assets/8.jpg) 
        - 从机在拉高BVALID之前必须等待AWVALID、AWREADY、WVALID和WREADY以及WLAST  
        - 其他同AXI3

### 3.3.3. Legacy considerations

- AXI4&AXI5较AXI3的区别  
    Slave在接收正确的地址之前不返回给Master写响应信号

## 3.4. Transaction structure

### 3.4.1. Address structure  
    
A burst must not cross a 4KB address boundary.  

- **Burst length**  
    - **Definition**  
        - *AxLEN[3:0]* for AXI3，*AxLEN[7:0]* for AXI4 
          - *ARLEN[7:0]*，for read transfers
          - *AWLEN[7:0]*，for write transfers  
        - Burst_length = AxLEN + 1  
        
    - **Burst lengths supported**  
        1、AXI3对所有突发类型均支持1-16次传输长度  
        2、AXI4对INCR类型支持1-256次传输长度，对其他类型仅支持1-16次传输长度

    - **Rules**  
        1、对于WRAP类型，长度必须是2、4、8或16  
        2、一次Burst传输不能跨越4KB边界  
        3、不支持提前终止Burst传输
        - 可以通过禁止进一步的写操作或者忽略后续数据 来达到减少传输次数，但是在此情况下仍然需要完成后续的传输。
        - 访问一些对读操作敏感的设备如FIFO等，忽略后续数据会造成数据丢失
        - 可将长Burst打成多个短Burst
        - 
- **Burst size**
    - **Definition**  
        1、*AxSIZE[2:0]*，表示在Burst传输中，一次传输可支持的最大字节数  
        2、*ARSIZE[2:0]*，for read transfers  
        3、*AWSIZE[2:0]*，for write transfers

    - **Encoding**  
        | AxSIZE[2:0] | Bytes in transfer |
        |-|-|
        |000|**1**|
        |001|**2**|
        |010|**4**|
        |011|**8**|
        |100|**16**|
        |101|**32**|
        |110|**64**|
        |111|**128**|
    - ***Burst大小不能超过数据总线宽度*** 

- **Burst type**
    - **Types**  
        ***FIXED***  
        传输过程中地址固定，用于FIFO访问 

        ***INCR***  
        增量突发，传输过程中，地址递增，增加量取决于AxSIZE的值 

        ***WRAP***    
        1、回环突发，和增量突发类似，但会在特定高地址的边界处回到低地址处。回环突发的长度只能是2,4,8,16次传输，传输首地址和每次传输的大小对齐。最低的地址与整个传输的数据大小对齐。回环边界等于（AxSIZE*AxLEN）  
        2、用于访问Cache line

    - **Definition**
        1、*AWBURST[1:0]*，for write tranfers  
        2、*ARBURST[1:0]*，for read tranfers  
        3、Encoding  
        | AxBURST[1:0] | Burst type |
        |-|-|
        |000|*FIXED*|
        |001|*INCR*|
        |010|*WRAP*|
        |011|*Reserved*|

- **Burst address**
    - **Variables**  
        - $Start\_Address$  
        首地址  
        - $Number\_Bytes$  
        每次传输可以支持的最大字节数  
        - $Data\_Bus\_Bytes$  
        在数据总线中的字节通道数  
        - $Aligned\_Address$  
        首地址的对其形式  
        - $Burst\_Length$  
        在一个Burst传输中总传输次数  
        - $Address\_N$  
        在一个Burst传输中第N次传输的地址  
        - $Wrap\_Boundary$  
        在WRAP Burst中的地址下边界   
        - $Lower\_Byte\_Lane$  
        在一次传输中的最低寻址字节的字节通道  
        - $Upper\_Byte\_Lane$  
        在一次传输中的最高寻址字节的字节通道 
        - $INT(x)$  
        向下取整 

    - **Equations**
        - *Address* 
            - 首地址  
              $Start\_Adress = AxADDR$
            - 数据包字节数  
              $Number\_Bytes = 2^{AxSIZE}$
            - 传输长度
              $Burst\_Length = AxLEN + 1$
            - 对齐地址
              $Aligned\_Address = (INT(Start\_Address / Number\_Bytes)) \times Number\_Bytes$
            - 第一次传输地址
              $Address\_1 = Start\_Address$
          
            - INCR burst和地址未到达边界的WRAP burst
                - 第N次传输地址
                  $Address\_N = Aligned\_Address + (N-1) \times Number\_bytes$
            
            - WRAP burst   
                - 回环边界  
                  $Wrap\_Boundary = (INT(Start\_Address / (Number\_Bytes \times Burst\_Length))) \times (Number\_Bytes \times Burst\_Length)$   
                - 当地址递增至$Wrap\_Boundary + (Number\_Bytes \times Burst\_Length)$  
                    - 当前传输地址  
                      $Address\_N = Wrap\_Boundary$
                    - 下一传输地址
                      $Address\_ = Start\_Adress + (N-1)\times Number\_Bytes - (Number\_Bytes \times Burst\_Length)$  
        - *Byte lane*
            - For first transfer
              - 最低寻址字节通道
                - $Lower\_Byte\_Lane = Start\_Address – (INT(Start\_Address / Data\_Bus\_Bytes)) \times Data\_Bus\_Bytes$
              - 最高寻址字节通道
                - $Upper\_Byte\_Lane = Aligned\_Address + (Number\_Bytes – 1) – (INT(Start\_Address / Data\_Bus\_Bytes)) \times Data\_Bus\_Bytes$
            - For other transfers
              - 最低寻址字节通道
                - $Lower\_Byte\_Lane = Address\_N – (INT(Address\_N / Data\_Bus\_Bytes)) \times Data\_Bus\_Bytes$
              - 最高寻址字节通道
                - $Upper\_Byte\_Lane = Lower\_Byte\_Lane + Number\_Bytes - 1$
            - Data is transferred on:
              - $DATA((8 \times Upper\_Byte\_Lane) + 7: (8 \times Lower\_Byte\_Lane))$  
        - *Transaction Container*
          - $Container\_Size = Number\_Bytes \times Burst\_Length$
          - For INCR bursts
            - $Container\_Lower = Aligned\_Adress$
            - $Container\_Upper = Aligned\_Adress + Container\_Size$
          - For WRAP bursts
            - $Container\_Lower = Wrap\_Boundary$
            - $Container\_Upper = Wrap\_Boundary + Container\_Size$

### 3.4.2. Pseudocode description of the transfers

```text
//DataTransfer()
//=============

DataTransfer(Start_Address, Number_Bytes, Burst_Length, Data_Bus_Bytes, Mode, IsWrite)
// Data_Bus_Bytes is the number of 8-bit byte lanes in the bus
// Mode is the AXI transfer mode
// IsWrite is TRUE for a write, and FALSE for a read

assert Mode IN {FIXED, WRAP, INCR};   
addr = Start_Address;     // Variable for current address    
Aligned_Address = (INT(addr/Number_Bytes) * Number_Bytes);    
aligned = (Aligned_Address == addr);    // Check whether addr is aligned to nbytes   
dtsize = Number_Bytes * Burst_Length;   // Maximum total data transaction size 

if mode == WRAP then        
    Lower_Wrap_Boundary = (INT(addr/dtsize) * dtsize);        
    // addr must be aligned for a wrapping burst        
    Upper_Wrap_Boundary = Lower_Wrap_Boundary + dtsize;    
for n = 1 to Burst_Length        
    Lower_Byte_Lane = addr - (INT(addr/Data_Bus_Bytes)) * Data_Bus_Bytes;        
    if aligned then            
        Upper_Byte_Lane = Lower_Byte_Lane + Number_Bytes - 1        
    else            
        Upper_Byte_Lane = Aligned_Address + Number_Bytes - 1 - (INT(addr/Data_Bus_Bytes)) * Data_Bus_Bytes;         
        
    // Peform data transfer        
    if IsWrite then            
        dwrite(addr, low_byte, high_byte);
    else            
        dread(addr, low_byte, high_byte);        
        
    // Increment address if necessary        
    if mode != FIXED then            
        if aligned then
            addr = addr + Number_Bytes;                
            if mode == WRAP then                    
                //WRAP mode is always aligned                    
                if addr >= Upper_Wrap_Boundary then 
                    addr = Lower_Wrap_Boundary;           
        else                
            addr = Aligned_Address + Number_Bytes;                
            aligned = TRUE;            // All transfers after the first are aligned    
return;


```

### 3.4.3. Data read and write structure

- **Write strobes**
  - *WSTRB[n]* 对应WDATA[(8n)+7:(8n)],每一位选通信号对应8位数据位，即1个字节
  - *WSTRB[n]* 为高表示WDATA[(8n)+7:(8n)]包含的数据有效，且仅当对应数据位中的数据有效时主机才能拉高WSTRB
  - WVALID为低时，WSTRB可以是任意值

- **Narrow transfers**
  - Burst Size小于总线位宽时即发生窄传输，进行窄传输时需要选择数据传输的字节通道(Byte Lane)
    - 对于增量和回环突发传输，每一次传输所用到的字节通道不尽相同
    - 对于固定突发传输，每一次传输用到的字节通道是一样的
  - 示例
    - 8-bit transfers on 32-bit bus  ![8-bit](README.assets/9.jpg)
    - 32-bit transfers on 64-bit bus ![32-bit](README.assets/10.jpg)

- **Byte invariance**
  - 即字节所在的位置与大小端定义无关。完全按照其地址所对应的偏移来决定其采用哪一个字节通道(byte lane)来传输，换句话说，在AXI中，认为所有的传输都是以byte为单位的，每次传输都是传了多少个byte
  - 字节不变性保证了大小端数据存储在同一个Memory中不会会互相影响

- **Unaligned transfers**
  - 使用低位地址线表示未对齐的起始地址。
  - **提供一个对齐的地址，并使用字节通道选通信号指示未对齐的起始地址。**

### 3.4.4. Read and write response structure

- **Response signals**
  - **读事务**
    - *RRSEP[1:0]*
    - 通过读数据通道返回给Master
  - **写事务**
    - *BRESP[1:0]*
    - 通过写响应通道返回给Master
  - **xRESP Encoding**
    | xRESP[1:0] | Response |
    |--|--|
    |00|*OKAY*|
    |01|*EXOKAY*|
    |10|*SLVERR*|
    |11|*DECERR*|

- **Response indication**
  - ***OKAY, normal access success***
    - 普通访问成功
    - 独占访问失败
    - 对不支持独占访问的Slave进行独占访问
  - ***EXOKAY, exclusive access success***
    - 独占访问成功
  - ***SLVERR, slave error***
    - 事务传输失败
    - **Slave发生错误的情况**
      - FIFO或缓存溢出
      - 传输大小不支持
      - 对只读区域进行写操作
      - Slave响应超时
      - 访问禁用或掉电功能
  - ***DECERR, decode error***
    - Interconnect无法解码

# 4. Transaction Attributes

## 4.1. Transaction types and attributes

- **Slave**
  - **Memory slave**
  - **Periphral slave**

- **Attributes**
  - **相关信号**
    - *AWCACHE*
    - *ARCACHE*
  - **作用**
    - 控制事务传输在系统中如何进行
    - 控制系统级缓存如何处理事务

## 4.2. AXI3 memory attribute signaling

### 4.2.1. Transaction attribute encodeing

- **Encoding** 
    <table>
        <tr>
            <th>AxCACHE[n]</th>
            <th>Value</th>
            <th>Transaction Attribute</th>
        </tr>
        <tr>
            <td rowspan="2">0</td>
            <td>0</td>
            <td><em>Non-bufferable</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Bufferable</em></td>
        </tr>
        <tr>
            <td rowspan="2">1</td>
            <td>0</td>
            <td><em>Non-cacheable</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Cacheable</em></td>
        </tr>
        <tr>
            <td rowspan="2">2</td>
            <td>0</td>
            <td><em>No Read-Allocate</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Read-Allocate</em></td>
        </tr>
        <tr>
            <td rowspan="2">3</td>
            <td>0</td>
            <td><em>No Write-Allocate</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Write-Allocate</em></td>
        </tr>
    </table>

- ***AxCACHE***
  - *AxCACHE[0], Bufferable(B)bit*
    - 此位拉高时，该传输事务在到达目的地之前可以被Interconnect或者任何AXI组件缓存、延迟若干周期
    - 一般只用于写事务 
  - *AxCACHE[1], Cacheable(C)bit*
    - 拉高时允许对传输事务进行分配，其他相关信息由RA和WA提供
    - 最终目的地的事务属性不必与原事务属性匹配
      - 写事务可以聚合
      - 读事务可以预取数据或者一次取数据可以用于多次传输
  - *AxCACHE[2], Read-Allocate(RA)bit*
    - 读分配策略
  - *AxCACHE[3], Write-Allocate(WA)bit*
    - 写分配策略

## 4.3. AXI4 changes to memory attribute signaling

### 4.3.1. *AxCACHE[1], Modifiable*

- **Non-modifiable transactions**
  - *AxCACHE[1]* 为低表示传输事务传输特性**不可修改**
  - 传输事务不能被分割或聚合，但当突发长度大于16时可以被分割，分割后的事务仍然遵守本小节的规则
  - 以下参数不可修改
    |Parameter|Signals|
    |--|--|
    |Transfer Address|*AxADDR、AxREGION*|
    |Burst size|*AxSIZE*|
    |Burst length|*AxLEN*|
    |Burst type|*AxBURST*|
    |Lock type|*AxLOCK*|
    |Protection type|*AxPORT*|
  - *AxCACHE* 仅可修改Bufferable位使其属性从Bufferable变为Non-bufferable
  - 事务ID和QoS值可以修改
  - **注意**
    当Burst size大于数据总线位宽时，事务可不遵循上述规则

- **Modifiable transactions**
  - *AxCACHE[1]* 为高表示传输事务传输特性**可修改**
  - 传输事务可以分割或聚合
  - 读传输事务中，可以预取相邻地址上的读数据，目的地址可以返回比主机需求更多的数据
  - 写传输事务中，可以访问比需求地址跨度更大的区域，其中使用 *WSTRB* 信号防止更新无关地址
  - 可修改的参数
    - **Transfer address**
    - **Burst size**
    - **Burst length**
    - **Burst type**
    - **Transaction ID**
    - **QoS value**
  - 不可修改的参数
    - **Lock type**
    - **Protection Type**
  - **内存属性也可修改**，只是保证其他组件对事务的可见性不被降低，例如阻止事务传输到指定的位置或更改在缓存中查找事务的需求。另外对同一地址范围内的所有事务，对内存属性的任何更改必须保持一致
  - **注意：两种情况下，事务属性不可修改**
    - 修改地址后若超过KB边界
    - 导致对单拷贝原子大小区域的单次访问被作为多次访问执行

### 4.3.2. Updated meaning of Read-Allocate and Write-Allocate

- **Redefining**
  - **读传输事务**
    - *WA*位拉高表示当前地址的数据因为写传输事务或者其他主机的传输已经缓存在Cache中
  - **写传输事务**
    - *RA*位拉高表示当前地址的数据因为读传输事务或者其他主机的传输已经缓存在Cache中

- **Cache Allocate**
  - *AWCACHE bit allocations* ![Write-Allocate](README.assets/11.jpg)
  - *ARCACHE bit allocations* ![Read-Allocate](README.assets/12.jpg)

## 4.4. Memory types

### 4.4.1. Memory type encoding

<table>
    <tr>
        <th>ARCACHE[3:0]</th>
        <th>AWCACHE[3:0]</th>
        <th>Memory type</th>
    </tr>
    <tr>
        <td>0b0000</td>
        <td>0b0000</td>
        <td><em>Device Non-bufferable</em></td>
    </tr>
    <tr>
        <td>0b0001</td>
        <td>0b0001</td>
        <td><em>Device Bufferable</em></td>
    </tr>
    <tr>
        <td>0b0010</td>
        <td>0b0010</td>
        <td><em>Normal Non-cacheable Non-bufferable</em></td>
    </tr>
    <tr>
        <td>0b0011</td>
        <td>0b0011</td>
        <td><em>Normal Non-cacheable Bufferable</em></td>
    </tr>
    <tr>
        <td>0b1010</td>
        <td>0b0110</td>
        <td><em>Write-Through No-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1110(0b0110)</td>
        <td>0b0110</td>
        <td><em>Write-Through Read-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1010</td>
        <td>0b1110(0b1010)</td>
        <td><em>Write-Through Write-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1110</td>
        <td>0b1110</td>
        <td><em>Write-Through Read and Write-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1011</td>
        <td>0b0111</td>
        <td><em>Write-Back No-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1111(0b0111)</td>
        <td>0b0111</td>
        <td><em>Write-Back Read-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1011</td>
        <td>0b1111(0b1011)</td>
        <td><em>Write-Back Write-Allocate</em></td>
    </tr>
    <tr>
        <td>0b1111</td>
        <td>0b1111</td>
        <td><em>Write-Back Read and Write-Allocate</em></td>
    </tr>
</table>
 
- **表中未出现的值均保留**

### 4.4.2. Memory type requirements

- **Device访问**
  - 在访问 Device 时，*AxCACHE[3:1] = 3'b000*,传输事务在中间节点不能被修改。具体地说，不能预读数据（Prefetch read）和汇聚写数据（Merge write）。这是因为在访问非存储外设时，读写的是寄存器值，预取数据是没有必要的。而将不同的写事务聚集则容易造成预期之外的问题，比如会导致相邻寄存器操作的先后顺序无法满足。
  - 根据 *AxCACHE[0]* 决定 Device 访问是否可以被中间节点缓存，决定*bufferable*性质。
    - **Non-bufferable**，*AxCACHE[0] = 0*，不可以被中间节点缓存
    - **Bufferable**，*AxCACHE[0] = 1*，可以被中间节点缓存

- **Normal Non-cacheable访问**
  - Normal 访问指正常地访问存储介质，而不会查找缓存，*AxCACHE[3:1] = 3'b001*。Normal 非缓存访问中，中间组件可以对传输事务信息进行修改，支持写事务聚合。
  - 根据*AxCACHE[0]* 决定 normal 访问是否可以被中间节点缓存，决定bufferable性质。
    - **Non-bufferable**，*AxCACHE[0] = 0*，不可以被中间节点缓存
    - **Bufferable**，*AxCACHE[0] = 1*，可以被中间节点缓存

- **Write-Through访问**
  - *Write-through* 指缓存的写入策略为直写，即数据写入缓存的同时，也写入主存储中。此时*AxCache[1:0] = 2'b10*，即中间组件可以修改传输事务，实现写聚合与读预取。*AxCache[0]* 置低，每个写事务最终必须写入目的地址。
  - 此类访问启用缓存，读数据可以来自缓存，并且每次读写操作都需要查找缓存，寻找匹配表项。
  - 根据*AxCache[3:2]* 不同，从图中得到共有 4 种情况，分别代表不同的分派提示。不过这都只是代表处理器从性能出发给出的建议，由缓存控制器视情况执行。
    - **No-Allocate**，*ARCACHE[3:2] = 2'b10*、*AWCACHE[3:2] = 2'b01*，代表建议不要为该事务分派缓存空间。
    - **Read-Allocate**，*ARCACHE[3:2] = 2'bx1*、*AWCACHE[3:2] = 2'b01*，代表建议为读事务分派缓存，但不建议为写事务分派缓存。
    - **Write-Allocate**，*ARCACHE[3:2] = 2'b10*、*AWCACHE[3:2] = 2'b1x*，代表建议为写事务分派缓存，但不建议为读事务分派缓存。
    - **Read and Write-Allocate**，*ARCACHE[3:2] = 2'b11*、*AWCACHE[3:2] = 2'b11*，代表建议为读写事务均分派缓存。

- **Write-Back访问**
  - *Write-Back* 指缓存的写入策略为写回，数据仅写入缓存，修改的缓存只在被替换时写入主存储。
  - *AxCache[1:0] = 2'b11*，即中间组件可以修改传输事务，并且进行缓存。
  - 与 Wirte-through 访问相比，区别在于由于写回策略中并不是每次写事务后都需要更新主缓存，因此无需将每个写事务传输至其本来的目的地（即主存储）。
  - 和*Write-Through* 相同，根据*AxCache[3:2]* 不同，从图中得到共有 4 种情况，分别代表不同的分派提示。不过这都只是代表处理器从性能出发给出的建议，由缓存控制器视情况执行。
    - **No-Allocate**，*ARCACHE[3:2] = 2'b10*、*AWCACHE[3:2] = 2'b01*，代表建议不要为该事务分派缓存空间。
    - **Read-Allocate**，*ARCACHE[3:2] = 2'bx1*、*AWCACHE[3:2] = 2'b01*，代表建议为读事务分派缓存，但不建议为写事务分派缓存。
    - **Write-Allocate**，*ARCACHE[3:2] = 2'b10*、*AWCACHE[3:2] = 2'b1x*，代表建议为写事务分派缓存，但不建议为读事务分派缓存。
    - **Read and Write-Allocate**，*ARCACHE[3:2] = 2'b11*、*AWCACHE[3:2] = 2'b11*，代表建议为读写事务均分派缓存。
 
## 4.5. Mismatched memory attributes

### 4.5.1. Mismatched memory attributes

- 所谓**内存属性不匹配**指的是多个Master可采用不同的内存属性去访问同一内存区域。但必须遵循以下准则：
  - 所有访问同一内存区域的的Master需要考虑任何层次内存区域的缓存一致性问题:
    - 访问**不可缓存**地址区域：所有传输事务的AxCACHE[3:2]必须为2'b00;
    - 访问**可缓存**地址区域：所有传输事务的AxCACHE[3:2]必须**不**为2'b00。
- 不同的Master可以采用不同的**缓存分派策略**
- 访问具有**正常不缓存**属性的寻址区域时，可采用**Device**访问，即可把此块存储区域视为外设
- 如果寻址区域具有Bufferable属性，则任何Master都可以使用不允许进行缓冲行为的事务来访问

### 4.5.2. Changing memory attributes

- 一些特殊的内存区域可以修改内存属性，比如从**直写**改为**写回**。
- 执行更改时需要合理的流程：
  - 所有主机停止访问该内存区域
  - 单个主机执行缓存维护操作
  - 所有主机采用新的属性重新访问

## 4.6. Transaction buffering

### 4.6.1. Write transactions

- 对以下内存类型，写事务具有相同的要求：即事务的响应可以来自中间节点，而非目的地；另外写事务必须对目的地及时可见（写事务必须最终到达目的地，而不能一直缓存在Buffer中）。
  - **Device Bufferable**
  - **Normal Non-cacheable Bufferable**
  - **Write-Through**

- 可以保存和合并写事务的**中间缓冲区**必须确保事务不会无限期地保留在其缓冲区中。 例如，合并写事务不得重置确定何时将写事务传输到最终目的地的机制。

### 4.6.2. Read transactions

- 对于不同的内存类型，读事务具有不同的要求：
  - **Device Bufferable：** 读数据必须从目的地获取
  - **Normal Non-cacheable Bufferable：** 读数据要么从目的地获取，要么从目的地相同的写事务中获取。从写事务获取读数据时必须保证写事务是最新版本，同时由此得来的数据不能被缓存
  - **Write-Through：** 读数据可以从中间的Cache中获得

- 可以响应事务的**中间缓冲区**必须确保对*Normal Non-cacheable Bufferable* 内存的任何读事务最终都将到达其目的地。这就意味着中间缓冲区对事务的缓冲不可能一直持续下去。该协议未定义任何机制来确定用于转发读取事务的数据可以保留多长时间。但是，在这种机制下，读取数据的行为一定不能重置数据超时时间。

## 4.7. Access permissions

### 4.7.1. Signals

- **AxPORT**
    为了阻止恶意程序越权访问的关键外设，AXI 协议设计了访问权限控制信号 AxPROT，配合其他安全机制，限定不同应用的访问权限。
  - ARPORT[2:0]，定义了读事务的访问权限
  - AWPORT[2:0]，定义了写事务的访问权限

- **Protection encoding**
    <table>
        <tr>
            <th>AxPORT[n]</th>
            <th>Value</th>
            <th>Function</th>
        </tr>
        <tr>
            <td rowspan = '2'>0</td>
            <td>0</td>
            <td><em>Unprivileged access</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Privileged access</em></td>
        </tr>
        <tr>
            <td rowspan = "2">1</td>
            <td>0</td>
            <td><em>Secure access</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Non-secure access</em></td>
        </tr>
        <tr>
            <td rowspan = "2">2</td>
            <td>0</td>
            <td><em>Data access</em></td>
        </tr>
        <tr>
            <td>1</td>
            <td><em>Instruction access</em></td>
        </tr>
    </table> 

### 4.7.2. Protection attributes

- **Unprivileged acces**
- **Privileged acces**
- **Secure acces**
- **Non-secure acces**
- **Data acces**
- **Instruction acces**

# 5. Transaction Identifiers

## 5.1. AXI transaction identifiers

- **Identifiers**
  - AXI提供事务标识符（ID）来标识必须按顺序返回给Master的单个事务。
  - 具有给定ID的所有事务必须按照特定的顺序进行传输，具有不同ID的事务则没有顺序上的限制。
  - 一个物理接口可以被多个逻辑接口复用，从而实现事务的乱序传输。但对于每个逻辑接口而言，事务的传输必须按照特定的顺序。
- **注意**
  - AXI协议并未要求必须使用ID。Master和Slave可以按照事务发出的顺序一次只处理一个事务。
  - Slave在作出响应时须在信号*BID* 或*RID*上要包含标志发出事务的Master的ID信息

## 5.2. ID signals

### 5.2.1. Channel transaction ID
<table>
    <tr>
        <th>Transaction channel</th>
        <th>Transaction ID</th>
    </tr>
    <tr>
        <td>Write address channel</td>
        <td><em>AWID</em></td>
    </tr>
    <tr>
        <td>Write data channel(AXI3 only)</td>
        <td><em>WID</em></td>
    </tr>
    <tr>
        <td>Write response channel</td>
        <td><em>BID</em></td>
    </tr>
    <tr>
        <td>Read address channel</td>
        <td><em>ARID</em></td>
    </tr>
    <tr>
        <td>Read data channel</td>
        <td><em>RID</em></td>
    </tr>
</table>

- *WID*仅在AXI3中支持，此时写地址和写数据通道的顺序是任意的。但是在AXI4中*WID*被取消，写数据通道只能紧接着写地址通道，因此就不存在*数据超前于地址*的传输，此种情况下，写地址和写数据通道的传输顺序是一致的。

### 5.2.2. Read data ordering

- Slave返回数据时必须确保RID值与响应的地址的ARID值相匹配
- 在具有不同目的Slave但是具有相同ARID值的读事务序列传输中，Interconnect必须确保主机是按照发送地址的顺序来获取读数据的
- 数据重排序
  - 读取数据的重排序深度取决于Slave中重排序缓存区的大小。
  - 按顺序处理所有事务的Slave的重排序深度为1。
  - 重排序深度相当于Slave的一个属性，是个静态值，由设计者指定。

### 5.2.3. Write data ordering

- Master发送数据的顺序必须和发送地址的顺序是一致的。
- Interconnect可以合并来自于不同Master的写传输事务，但是数据的传输必须遵循地址顺序。
- 在AXI3中就已经允许**写数据重排序**，但是在AXI4中并不建议使用写数据重排序。

### 5.2.4. Interconnect use of transaction identifiers

- 当Master和Interconnect相连时，Interconnect会向该主机端口的唯一标识符AWID、WID、ARID添加附加信息：
  - Master不必知道其他Master使用哪些ID值，因为Interconnect通过将主机号附加到原始标识符上，使每个Master使用的ID值唯一。
  - 从接口的ID标识符比主接口的ID标识符宽。
- 对于读数据，Interconnect使用RID标识符的附加位来确定读数据的目标主机端口。在将RID值传递到正确的主端口之前，Interconnect模块会删除RID标识符的附加位。
- 对于写响应，Interconnect使用BID标识符的附加位来确定写响应的目标主机端口。在将BID值传递到正确的主端口之前，Interconnect模块会删除BID标识符的附加位。

# 6. AXI Ordering Model

## 6.1. AXI ordering model overview

- AXI排序模型是基于事务标识符实现的，事务标识符通过*AWID*和ARID*发出。
- 同一通道上具有相同ID和目的地的事务请求要保持发出的顺序。
- 具有相同ID的事务响应要按照Master发出的顺序返回。
- 以下情况没有顺序限制：
  - 事务来自不同的Master
  - 读写事务之间
  - 具有不同ID的事务
  - 事务到达不同外设区域
  - 事务到达不同内存单元
- 如果Master要求无顺序保证的事务按顺序执行，则其必须等待接收上一个事务的响应后才能发出下一个事务。

## 6.2. Memory locations and Peripheral regions

- AMBA的地址映射包括存储单元和外设区域：
  - **内存单元**：
    - 从内存单元读取一个字节时返回的是该字节位置的最新值
    - 向内存单元写入一个字节会将该位置的值更新为一个新值，该值是通过对该位置的后续读取获得的
    - 对任何内存单元的读写操作都不会影响其他单元
    - 内存单元的每个位置都提供“观察保证”
    - 内存单元的大小等于该组件的单拷贝原子性大小。
  - **外设区域**：
    - 从外设区域中读取数据时并不一定返回该位置的最新值
    - 向外设区域中写入数据时不一定会将该地址上的值更新为通过后续读取获得的新值
    - 访问外设区域中的地址可能会对该区域的其他地址造成影响
    - 每块外设区域都为外设提供了“观察保证”
    - 外设区域的大小是在设计外设时定义的，但无论设计多大它都代表一个从组件

## 6.3. Transactions and ordering

- 所谓事务就是对一个或多个地址的读写，访问的地址由*AxADDR*和相关的限定词决定，例如*AxPORT*中的*Non-Secure*位
  - 仅在访问相同的内存单元或外设区域之间提供排序保证
  - 外设区域的事务传输必须完全包含在该区域之内
  - 目的节点囊括多个内存单元的事务具有多个顺序保证

- 事务可以分为*Device*访问和*Normal*访问
  - *Device*
    - *AxCACHE[1] = 0*
    - 设备事务可以用来访问外设区域或内存单元
  - *Normal*
    - *AxCACHE[1] = 1*
    - 普通事务可以用来访问内存单元，不希望用于访问外设区域
    - 普通事务访问外设区域必须以符合协议的方式完成，但是具体结果还是要在设计Slave时实现

- 写事务可以分为*Bufferable*和*Non-Bufferable*
  - 写事务类型为*Bufferable*时，可以在到达目的节点之前返回事务响应
  - *Buffeable：AxCACHE[0] = 1*; *Non-Bufferable：AXCACHE[0] = 0*
 
## 6.4. Observation and completion definitions

### 6.4.1. Observation

- **外设区域**
  - 对于外设区域的访问，读写早到的设备读写事务DRW1受到晚到的设备设备读写事务DRW2的观察

- **内存单元**
  - 写事务W1对在其之后生效的写事务W2可见
  - 读事务R1从写事务W3获取数据，而写事务W2发生在W3之后，则R1对W2可见
  - 如果读事务R2从写事务W1或W3获取数据，且W3在W1之后，则W1对R2可见
  - 读写事务类型可以是*Device*或*Normal*

### 6.4.2. Completion

- **Write completion response**
  - 当*BVALID*和*BREADY*有效时，给出关联的*BRESP*握手信号的周期即标志着写事务的完成
- **Read conpletion response**
  - 当*RVALID*、*RLAST*和*RREADY*有效时，给出最后关联的*RDATA*握手信号的周期即标志着读事务的完成

## 6.5. Master ordering guarantees

- **顺序模型保证**
  - 收到完成响应之前的可见性保证
  - 来源于完成响应的可见性保证
  - 响应顺序保证

### 6.5.1. Guarantees before a completion response is received

- 下面所述**顺序保证**的情况均是来源于同一Master，使用相同ID：
  - 访问同一外设区域的两个设备写事务DW1和DW2，DW1先于DW2发出，则顺序模型保证DW1是先于DW2到达指定区域
  - 设备读事务同设备写事务
  - 访问同一内存单元的两写事务，前一事务对后一事务可见
  - 写事务W1对读事务R2可见，W1仍然对在R2之后发出且与R2访问同一内存单元的读事务可见
- 上述保证意味着访问同一内存单元的设备访问和普通访问之间存在顺序保证

### 6.5.2. Guarantees from a completion response

- 完成响应保证以下条件：
  - 对读请求的完成响应保证了对来自任何Master的后续读取或写入请求均可见。
  - 对写请求的完成响应保证了对来自任何Master的后续读取或写入请求均可见。这种可见性是多拷贝原子系统的要求。
- 包含符合ARM架构体系的处理器的系统必须是具有多拷贝原子性。
- 可缓冲写事务的响应可以由中间节点发出。它不能保证在目的节点上已完成写入，但是对之后的事务可见。

### 6.5.3. Response ordering guarantees

- 事务响应遵循以下顺序：
  - 由同一Master发出并且具有相同ID的两个读事务，读响应的返回顺序与读请求的发出顺序一致。
  - 由同一Master发出并且具有相同ID的两个写事务，写响应的返回顺序与写请求的发出顺序一致。

## 6.6. Ordering requirements

### 6.6.1. Slave ordering requirments

- **外设区域**
  - 访问外设区域的事务的执行顺序在设计外设时确定。
  - 一般上述的执行顺序是与事务到来的顺序匹配的。

- **内存单元**
  - 访问同一内存单元并且具有相同事务ID的写事务，Slave按照接收的顺序进行处理。
  - 访问同一内存单元的写事务，Slave在接收事务W2之前就已经给出事务W1的响应时，W1的顺序必须排在W2之前。
  - 访问同一内存单元的读写事务W1和R2，Slave在给出W1的响应之后接收R2，则W1必须排在R2之前。
  - 访问同一内存单元的读写事务R1和W2，Slave在给出R1的响应之后接收W2，则R1必须排在W2之前。

- **响应顺序**
  - 具有相同事务ID的读写事务，返回响应的顺序按照接收的顺序
 
### 6.6.2. Interconnect ordering requirments

- **互联组件的属性**
  - 在一个端口上接收到请求，该请求在另一个端口上被发出或被响应。
  - 在一个端口上接收到响应，该响应在另一个端口上被发出或被使用。

- **互连发出请求或响应时，遵循以下条件**
  - 访问同一或者具有重叠的内存单元并且具有相同事务ID的读写事务，请求的发出顺序同接收顺序。
  - 访问同一外设区域且具有相同事务ID的读写事务，请求的发出顺序同接收顺序。
  - 具有相同事务ID的读写事务，请求的发出顺序同接收顺序。
- **注意**
  - 当互连表现为一个从组件时，此时遵循Slave的顺序要求。
  - 任何关于事务ID值的操作都必须确保维持原始ID值的顺序要求。

## 6.7. Response before the endpoint

- **Early response**
  - 为了提高系统性能，一些事务可以由中间组件响应，这种行为被称为*Early response*。这种情况下中间组件必须保证事务的可见性和保序性。

### 6.7.1. Early read response

- 对于普通读事务，如果中间组件的本地内存中的数据相对于之前对相同或重叠地址的写入是最新的，则该中间组件可以用本地内存中的数据进行响应。
- 中间组件按必须遵循ID排序规则，具有相同ID的事务返回响应时，必须按照接收时的顺序。

### 6.7.2. Early write response

- 对于可缓冲写事务，对下游没有观察者的事务，中间组件可以发送*Early write response*。这种情况下中间组件可以将数据备份到本地缓存中，但在丢弃该数据之前需要将事务传输到下游。
- 中间组件按必须遵循ID排序规则，具有相同ID的事务返回响应时，必须按照接收时的顺序。
- 发送*Early write response*之后，在事务传输到下游并收到写响应之前，中间组件要保证事务的保序性和可见性。
- 在发送*Early write response*与接收到下游的写响应期间，中间组件必须保证以下情况：
  - 如果对普通事务给出*Early write response*，则访问相同或具有重叠的内存单元的后续事务都须排在该事务之后。
  - 如果对设备事务给出*Early write response*，则访问相同外设区域的后续事务都须排在该事务之后。
- 当为可缓冲的设备事务提供*Early write response*，中间组件传播该写事务时不依赖于其他事务。中间组件在传播先前的设备写事务之前不能等待其他读写事务的到来。

## 6.8. Ordered write observation

### 6.8.1. Ordered write observation

- 为了提高支持不同排序模型的接口协议的兼容性，从接口可以为写事务提供更有力的顺序保证。 更有力的顺序保证被称为*Ordered write observation*。
- *Ordered write observation*保证来自同一Master并且具有相同ID的事务，前一事务对后一事务可见。
- 使用*Producer-Consumer*顺序模型的Master与具有*Ordered write observation*的从接口连接时，Master发送写事务时不须等待之前的事务响应。

### 6.8.2. Ordered_Write_Observation 
- *Ordered_Write_Observation*属性用来定义接口是否提供*Ordered write observation*
  - **TRUE**：提供
  - **FALSE**：不提供
  - 默认为*不提供*

# 7. Atomic Accesses

## 7.1. Single-copy atomicity size

### 7.1.1. Single-copy atomicity size

- AXI4协议引入了单拷贝原子大小的概念。该术语定义事务自动更新的最小字节数。AXI4协议要求的事务大小大于单拷贝原子性大小，必须以至少单拷贝原子性大小的块更新内存。
- 注意
  - 原子性并没有定义数据更新的确切时间。必须确保的是，没有任何Master能够观察到原子数据的部分更新形式。例如，在许多系统中，数据结构（例如链表）由32位原子元素组成。对这些元素之一进行原子更新需要同时更新整个32位值。不接受任何Master一次只观察16位的更新，然后再观察其他16位的更新是不可接受的。
- 更复杂的系统需要支持更大的原子元素，尤其是64位原子元素，以便Master可以使用基于这些更大原子元素的数据结构进行通信。
- 系统中支持的单拷贝原子性大小很重要，因为给定通信中涉及的所有组件都必须支持所需的原子元素大小。如果两个Master通过一个互连和一个Slave进行通信，则所有涉及的组件必须确保以原子性处理所需大小的事务。
- AXI4协议不要求特定的单拷贝原子性大小，并且系统可以设计为支持不同的单拷贝原子性大小。
- 不同组的组件可以具有不同的单拷贝原子性大小，以在组内进行通信。 在AXI4中，术语“单拷贝原子组”描述了可以以特定原子性进行通信的一组组件。 例如，图7-1显示了一个系统，其中：
  - 处理器，数字信号处理器（DSP），DRAM控制器，DMA控制器，外设，SRAM存储器和相关的互连位于32位单拷贝原子组中。
  - 处理器，DSP，DRAM控制器和相关的互连也位于64位单拷贝原子组中。
  ![图7-1](README.assets/13.jpg "图7-1")
- :grey_question:事务的原子性保证永远不会大于其起始地址的对齐形式。例如，未与8字节边界对齐的64位单拷贝原子组中的突发传输没有任何64位单拷贝原子保证。
- 与事务相关的字节选通信号不影响单拷贝原子性的大小。

### 7.1.2. Multi-copy write atomicity 

- **Multi_Copy_Atomicity**
  - **True**：支持多拷贝原子性
  - **False**：不支持多拷贝原子性
  - 默认不支持

## 7.2. Exclusive accesses
## 7.3. Locked accesses
## 7.4. Atomic access signaling
