- 程序都是init程序的子孙后代
- readelf -l 与 pmap 【pid】查看进程内存分布；哪些段只读、读写、可执行；地址空间布局随机化
  ```
  readelf -l ./demo
  
  Elf file type is DYN (Position-Independent Executable file)
  Entry point 0x7c0
  There are 9 program headers, starting at offset 64
  
  Program Headers:
    Type           Offset             VirtAddr           PhysAddr
                   FileSiz            MemSiz              Flags  Align
    PHDR           0x0000000000000040 0x0000000000000040 0x0000000000000040
                   0x00000000000001f8 0x00000000000001f8  R      0x8
    INTERP         0x0000000000000238 0x0000000000000238 0x0000000000000238
                   0x000000000000001b 0x000000000000001b  R      0x1
        [Requesting program interpreter: /lib/ld-linux-aarch64.so.1]
    LOAD           0x0000000000000000 0x0000000000000000 0x0000000000000000
                   0x0000000000000acc 0x0000000000000acc  R E    0x10000
    LOAD           0x000000000000fd68 0x000000000001fd68 0x000000000001fd68
                   0x00000000000002c8 0x00000000000002d0  RW     0x10000
    DYNAMIC        0x000000000000fd78 0x000000000001fd78 0x000000000001fd78
                   0x0000000000000200 0x0000000000000200  RW     0x8
    NOTE           0x0000000000000254 0x0000000000000254 0x0000000000000254
                   0x0000000000000044 0x0000000000000044  R      0x4
    GNU_EH_FRAME   0x00000000000009dc 0x00000000000009dc 0x00000000000009dc
                   0x000000000000003c 0x000000000000003c  R      0x4
    GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                   0x0000000000000000 0x0000000000000000  RW     0x10
    GNU_RELRO      0x000000000000fd68 0x000000000001fd68 0x000000000001fd68
                   0x0000000000000298 0x0000000000000298  R      0x1
  ```
- 通过挂载宿主机程序到虚拟机，就可以使用多样工具
<img width="2146" height="1194" alt="image" src="https://github.com/user-attachments/assets/af7fb86d-d059-4e25-9981-ffa3a1f3d444" />
<img width="1110" height="746" alt="image" src="https://github.com/user-attachments/assets/76d0a0b7-40bd-40e8-92f3-ba508970e036" />
- 打印main函数第一条指令
<img width="2060" height="1130" alt="image" src="https://github.com/user-attachments/assets/977f735a-cde5-459a-bf83-a8d992637359" />

