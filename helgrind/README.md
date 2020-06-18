# Reading notes of helgrind

## hg_addrdescr.c
### 1. 函数：`HG_(describe_addr)`经由宏展开为`vgHelgrind_describe_addr`，函数签名：`void vgHelgrind_describe_addr(DiEpoch ep, Addr a, AddrInfo* ai)`

