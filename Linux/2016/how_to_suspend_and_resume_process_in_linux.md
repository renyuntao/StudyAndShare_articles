# 如何在Linux中暂停和恢复一个进程?              
<!--
2016-09-04
--><br /><br />              

在某些情况下，你可能希望临时暂停一个进程，例如，你正在使用 `mpg123` 听音乐，这时，你想在一个翻译软件中听一个英文单词的发音，为了避免干扰，你可能想临时停止播放音乐，即暂定 `mpg123` 这个进程，等听完英文单词的发音之后，再重新恢复播放音乐，即恢复 `mpg123` 这个进程; 很多人遇到这种情况，可能会直接结束 `mpg123` 这个进程，等听完英文单词的发音之后，再重新运行 `mpg123`, 然而这样操作会十分麻烦，并且重新运行 `mpg123`, 音乐也不会从上次退出时的位置继续播放。因此，暂停，之后再恢复一个进程是一个更好的选择，下面来分别介绍暂定和恢复一个进程的方法。               

## 暂定一个进程        

要暂停一个进程，需要用到 `kill` 这个命令，不要被 `kill` 的名字所迷惑，`kill` 不仅仅可以用来杀死进程(虽然这是它最常用的功能)，它还有许多其他的功能。`kill` 通过向特定的进程发送不同的信号来实现对进程的不同控制，你可以在终端执行如下命令来查看所有 `kill` 可以发送的信号:             
         
```bash
$ kill -l
```
在我的电脑上，输出结果如下:            
        
```
1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP     
6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1     
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM     
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP     
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ     
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR     
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3     
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8     
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13     
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12     
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7     
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2     
63) SIGRTMAX-1	64) SIGRTMAX     
```

关于常用信号的具体含义，你可以在man手册的第7章节中查看关于 **signal** 的含义:              
     
```bash
$ man 7 signal
```
下面回到正题，要暂停一个进程，我们需要使用 `kill` 向特定的进程发送 **SIGSTOP** 或 **SIGTSTP** 信号，这两个信号都可以用来暂停一个进程。假如电脑上正在运行 `mpg123` 这个进程，则可以使用如下的命令来暂停 `mpg123`:              
         
```bash
$ kill -STOP `pigof mpg123`
```
或           
       
```bash
$ kill -TSTP `pidof mpg123`
```

另外，在 `$ kill -l` 的输出结果中，我们看到每一个信号名都有一个相对应的数字，例如信号 **SIGHUP** 对应数字1, 信号 **SIGSTOP** 对应数字 19。在使用 `kill` 来向进程发送信号时，我们也可以不使用信号名，而使用对应的数字来告诉 `kill` 发射对应的信号。因此，你也可以执行下面的两条命令来暂停进程 `mpg123`:    

```bash
$ kill -19 `pidof mpg123`
```
或           

```bash
$ kill -20 `pidof mpg123`
```

**注意:**              
          
- 上面命令中的 ` 是反引号，即 **Esc** 键下方的那个键，而不是单引号。                 
- 上面的命令是以 `mpg123` 这个进程作为例子的，如果你想要暂停某个进程，需要使用要暂停的进程的进程名来替换上面命令中的 `mpg123`            

### SIGSTOP 和 SIGTSTP 信号的区别            

虽然 `SIGSTOP` 和 `SIGTSTP` 信号都可以用来暂定一个进程, 但二者还是有一些区别的，限于本文篇幅，在此不再对此赘述，如果你对此感兴趣，可以通过下面的链接来查看二者的区别:            
        
**[What's the difference between SIGSTOP and SIGTSTP?](https://stackoverflow.com/questions/11886812/whats-the-difference-between-sigstop-and-sigtstp)**           


-------------------------------------

## 恢复一个进程                

上面使用 `kill` 来暂停了进程 `mpg123`，下面来恢复这个进程。要恢复一个进程，仍然需要用到 `kill` 这个命令，只是这次 `kill` 发送的信号不同而已。如果你跟着上面的讲解进行了操作，那么，现在你的电脑上有一个被暂停了的 `mpg123` 进程，要恢复这个进程，你可以执行如下的命令:               
          
```bash
$ kill -CONT `pidof mp123`
```
这次， `kill` 发送的是 `SIGCONT` 信号，要记住这个信号很简单，你只要想一下 **Continue**(继续) 这个单词就行了。               
        
同样，你可以在上述命令中使用 **SIGCONT** 这个信号所对应的数字来替代信号名 `CONT`:               
         
```bash
$ kill -18 `pidof mpg123`
```

**注意:**              
          
- 上面命令中的 ` 是反引号，即 **Esc** 键下方的那个键，而不是单引号。                 
- 上面的命令是以 `mpg123` 这个进程作为例子的，如果你想要暂停某个进程，需要使用要暂停的进程的进程名来替换上面命令中的 `mpg123`            

---------------------------------------

## 结束语                 

在一些情况下，我们需要去临时暂停一个进程，之后再重新恢复这个进程。这比直接去杀死一个进程，之后在重新运行这个进程要好很多。本文介绍了如何使用 `kill` 命令来暂停和恢复一个进程, 希望这篇文章可以使你能够更加优雅地使用Linux这个十分强大的系统。                

<!--
Reference:        
https://unix.stackexchange.com/questions/2107/how-to-suspend-and-resume-processes                  
https://stackoverflow.com/questions/11886812/whats-the-difference-between-sigstop-and-sigtstp           
-->
