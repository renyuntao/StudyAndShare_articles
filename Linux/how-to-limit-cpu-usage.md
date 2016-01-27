# Linux:如何限制进程的CPU使用率?           
2016-01-25 <br /> <br />       
&nbsp;&nbsp;&nbsp;当有一些比较耗时的作业时，为了避免影响正常的工作学习，我们通常会将这些比较耗时的作业放到后台去运行,然而有些作业十分耗费CPU，将这些作业直接放到后台去运行，会影响到前台的工作学习。解决的方法有两个，一个是直接限制这个作业的CPU使用率，另一种方法是通过修改作业的优先级来降低该作业的CPU使用率.实现这两种方法需要分别用到两个工具，分别是 `cpulimit` 和 `nice/renice`,下面将介绍这两个工具的用法(**以 Ubuntu 系统为例**).            
<br />           
## cpulimit
`cpulimit` 默认在 Ubuntu 系统中是没有安装的，你可以通过在终端执行如下命令来安装 `cpulimit`:          

    $ sudo apt-get update
    $ sudo apt-get install

安装好 `cpulimit` 后，为了测试，我写了一个比较耗费CPU的程序:           

    #include<iostream>
    int main()
    {
    	while(1);
    	return 0;
    }

上述程序保存在 `example.cxx` 文件中，接下来编译链接生成一个名为 `example` 的可执行文件:         

    $ g++ -c example.cxx
    $ g++ example.o -o example

之后在不使用 `cpulimit` 的情况下将 `example` 放到后台去运行:          

    $ ./example &

接下来使用 `top` 来观察该程序的运行情况:            

    $ top

在 `top` 显示的界面中，你会看到类似下面的内容:            

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND     
    3164 ryt       20   0    3352    612    520 R 100.0  0.0   0:28.15 example

从显示的结果可以看到，`example` 进程的CPU使用率已经达到了 `100%` .先将这个 `example` 进程 `kill` 掉，接下来我们将使用 `cpulimit` 来限制进程的CPU使用率，假如我想要将进程的CPU使用率限制在 `30%` 以内，则可以按照下面的步骤操作:            

1. 将 `example` 放到后台去运行:          

        $ ./example &

2. 使用 `cpulimit` 来限制 `example` 进程的CPU使用率:            

		$ cpulimit -l 30 -p $!
	
	上述命令中，`-l` 参数后面接想要限制的CPU使用率,本例中是30,亦即 `30%` ，`-p` 参数后面接想要限制的进程的PID(进程ID)。`$!` 在Linux中是有特殊含义的		，它表示的是上一条命令的PID，本例中 `$!` 就表示 `example` 进程的PID。           

执行完上述操作后，可以在再次执行 `top` 命令来查看 `example` 进程的运行情况:           

	$ top 

在 `top` 显示的界面中，你会看到类似下面的内容:       

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND     
    3164 ryt       20   0    3352    612    520 R 27.7  0.0   0:28.15 example

可以看到 `example` 进程的CPU使用率为 `27.7%`.          
**注意: 有些时候，`example` 进程的CPU使用率会超过 `30%` ，该情况通常发生在多核CPU的情况下.如果你的CPU有四个核，那么进程实际的CPU使用率会在 `0% ~ 120%` 之间.因为 `cpulimit` 限制CPU的使用率只是针对单个核的.**               
你可以使用 `nproc` 来查看你的机器的CPU核数:            

    $ nproc
    4

可以看到我的机器上，CPU是4核的。               

### cpulimit的一些其他的参数
`cpulimit` 除了可以通过 `-p` 参数来指定要限制的进程外，还可以通过如下参数来指定:           

- **-e:** 该参数后面跟要限制的进程的名称。下面仍以限制 `example` 进程为例:          

		$ ./example &
		$ cpulimit -l 30 -e example

- **-P(大写):** 该参数通过后跟可执行文件的绝对路径来指定要限制的进程，仍以 `example` 进程为例:         

		$ ./example &
		$ cpulimit -l 30 -P /path/to/example

`cpulimit` 还有很多其他的参数，你可以通过 `man cpulimit` 来了解更多.             

----------
## nice/renice
`nice` 和 `renice` 是通过改变进程的优先级来限制进程的CPU使用率的。Linux中，进程优先级通过设置 `NICE` 值来改变。一个进程的 `NICE` 取值范围是从 `-19`(优先级最高)到 `20`(优先级最低)。多数情况下，一个普通用户启动一个进程的默认优先级为0,下面仍以 `example` 为例:           

    $ ./example &   

使用 `top` 命令查看 `example` 进程的运行情况:           

	$ top          

在 `top` 的显示界面中你可以看到类似下面的内容:            

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND     
    3164 ryt       20   0    3352    612    520 R 100.0  0.0   0:28.15 example

其中 `NI` 列表示的是进程的 `NICE` 值，可以看到其值为0.            
如果在执行命令时加上 `nice` ，会使进程的 `NICE` 值改变为10(优先级降低),示例如下:         

    $ nice ./example
    $ top

在 `top` 显示的界面中，你会看到类似下面的内容：       

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND     
    3164 ryt       30   10    3352    612    520 R 100.0  0.0   0:28.15 example

可以看到进程的 `NICE` 值发生了改变.           
通过 `nice` 的 `-n` 参数可以具体指定进程的 `NICE` 值，比如要将 `example` 进程的 `NICE` 值设置为12,可以这样做:              

	$ nice -n 12 ./example

此外，在进程运行过程中，还可以使用 `renice` 来重新设置 `NICE` 值,示例如下:             

    $ ./example &
    [1] 3132
    $ renice 12 3132

上面的例子中，`3132` 是进程的ID，即PID。之后再通过 `top` 命令来查看 `example` 进程的运行情况，就会发现该进程的 `NICE` 值变为了12.        
**注意: 一个普通用户只能够将进程的 `NICE` 值调大，而要将进程的 `NICE` 值调小，则需要有root权限.**         
&nbsp;&nbsp;&nbsp;通过 `nice/renice` 来限制进程的CPU使用率的一个缺点就是无法准确的将一个进程的CPU使用率限制在一个具体的范围内，而且如果当前系统中只有一个耗费CPU的进程的时候，由于没有其他进程对CPU资源的争夺，因此即使将这个进程的 `NICE` 值设置为20(优先级最低)，该进程的CPU使用率也会达到100%.    

