# Reading notes of helgrind

## Basic structures:
1. `ExeContext`:
    - `chain`, struct _ExeContext*: 链表对象
    - `ecu`, Uint: 唯一确定ExeContext的标识符，**必须是4的倍数**，可用于内存检查中的原始Tracking。
    - `epoch`, DiEpoch(UInt, 为了和标准类型加以区分): 获取函数的定位点。*是不是可以理解为PC寄存器？*
    - `n_ips`, UInt: 指代Backtrace的数组深度。
    - `ips[0]`, Addr(unsigned long): 地址。

## hg_main.c
1. `Lock`:
    - `unique`, ULong: Persistence-Hashing
    - `magic`, UInt: 取值为0x6545b557(`LockN_MAGIC`, 正常非持续锁)，0x755b5456(`LockP_MAGIC`持续锁，复制)
    - `appeared_at`, ExeContext*: 在Helgrind中首先出现的位置。
    - `acquired_at`, ExeContext*: 给目标对象上锁的位置。（最近的从unlocked转换到locked的状态，必须和`.heldBy`同步）
    - `hbso`, SO*: 关联的同步对象
    - 