# Linux:如何查看你的电脑是否支持64位系统?             
2016-02-09   <br /><br />            
&nbsp;&nbsp;&nbsp;如今64位系统越来越多，很多人想要从32位系统转换为64位系统。然而并不是所有的电脑都支持64位系统的。你可以在64位的硬件上安装32位系统，但是反过来却不行。这里所说的硬件其实指的就是 `CPU`，因此判断一台电脑是否可以安装64位系统，就是要查看 `CPU` 是否支持64位指令。下面分享几种在Linux中查看 `CPU` 是否支持64位指令的方法.              

## 方法 #1: 使用 lscpu
`lscpu` 是一个命令行工具，它可以查看你的机器上的 CPU 信息。执行如下命令即可显示出CPU的信息:         

```bash
$ lscpu
Architecture:          i686
CPU op-mode(s):    32-bit, 64-bit
...
```

在输出的信息中，我们所关心的是第二行，即:           

> CPU op-mode(s): &nbsp;&nbsp;&nbsp; 32-bit, 64-bit

该条信息表示我的电脑上的CPU支持32为指令和64为指令，因此可以安装64位系统。如果该条信息中只显示了 `32-bit`，则表示CPU只支持32位指令，不能够安装64位系统。                     

## 方法 #2：查看文件 /proc/cpuinfo
文件 `/proc/cpuinfo` 中保存了本机的CPU信息，实际上，命令行工具 `lscpu` 也是通过读取该文件中的信息来输出结果的。下面使用 `cat` 来查看该文件中的内容:    

```bash
$ cat /proc/cpuinfo
```       

执行上述命令后，会输出下面的内容:                   

> processor   : 0             
> vendor_id   : GenuineIntel            
> cpu family  : 6             
> model       : 37             
> model name  : Intel(R) Core(TM) i3 CPU       M 370  @ 2.40GHz          
> stepping    : 5                
> microcode   : 0x2           
> cpu MHz     : 933.000           
> cache size  : 3072 KB            
> physical id : 0             
> siblings    : 4             
> core id     : 0             
> cpu cores   : 2             
> apicid      : 0              
> initial apicid  : 0             
> fdiv_bug    : no             
> f00f_bug    : no                
> coma_bug    : no               
> fpu     : yes                 
> fpu_exception   : yes                  
> cpuid level : 11                 
> wp      : yes                    
> flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant_tsc arch_perfmon pebs bts xtopology nonstop_tsc aperfmperf pni dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm pcid sse4_1 sse4_2 popcnt lahf_lm arat dtherm tpr_shadow vnmi flexpriority ept vpid                    
> ...               

&nbsp;&nbsp;&nbsp;输出的内容很多，但是我们所关心的只是输出结果中的 `flags` 部分的内容，即上面输出内容的最后一行。在该行中查找是否存在以 `_lm` 结尾的标志，该标志表示CPU支持 `ling mode`, `long mode`即表示64位.在我的电脑上，可以看到存在 `lahf_lm` 标志，因此支持64位系统。如果在未找到以 `_lm` 结尾的标志，则说明CPU只支持32位系统.                

## RAM的影响            
&nbsp;&nbsp;&nbsp;虽然CPU对是否可以安装64位系统起了决定性影响，但是RAM的大小也对是否适合安装64位系统有很大的影响，因为32位系统和64位系统所需的内存大小不同，64位系统相比32位系统要占用更多的内存。一般而言，如果内存小于2G，则最好安装32位系统，这样用起来会比较流畅。        
