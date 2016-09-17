# 在Ubuntu的终端中输入特殊符号            
<!--
2016-09-17
--> <br /><br />

有些时候，我们需要在文本中输入一些特殊符号，例如像 ☀ , ☂ , ∈ , ∉  这样的符号，我之前曾在 **[Linux:如何输入特殊符号?](how_to_type_special_character.html)** 一文中介绍了如何在 Linux 平台下利用 LibreOffice Writer 和 Vim 来输入特殊符号，但是这种方法有两个缺陷:    

- **只能在相应的软件中使用** 即只能在 LibreOffice Write 和 Vim 中输入特殊符号，而要在其他的地方输入特殊符号，则只能使用 **复制-粘贴** 的方法。                         
- **只能输入软件所提供的特殊符号** 无论是在 LibreOffice Write 中，还是在 Vim 中，你都只能输入这两个软件所提供的特殊符号，而如果你想要输入的特殊符号在这两个软件中没有提供，你就无能为力了。          

针对上面的两个问题，这篇文章将介绍另外两种输入特殊符号的方法, 使用下面将要介绍的两种方法，你可以在终端的命令行提示符后或任意的终端软件中输入特殊符号。                       

相关阅读: [Linux:如何输入特殊符号?](how_to_type_special_character.html)               

## 方法一(推荐)                       

使用这种方法，你首先需要知道所要输入的特殊符号所对应的十六进制表示，关于这一点，你可以参考 **[Unicode Table](http://www.tamasoft.co.jp/en/general-info/unicode.html)**, 那里有完整的Unicode符号与相应的十六进制表示的对照表。          

在你知道了要输入字符的的十六进制表示之后，你可以在终端中按下 **\<Ctrl> + \<Shift> + u** 组合键，之后释放这些键，输入你想要输入的符号的十六进制数，并按下回车键即可输入你想要输入的符号。例如, 你想要输入 ☯  这个符号(该符号所对应的十六进制表示为 262F), 你所要做的就是首先在终端按下 **\<Ctrl> + \<Shift> + u** 组合键并释放，之后输入 262F 并按下回车键，即可输入符号 ☯ 。          

### 优缺点分析       

使用该方法，优点是你可以在终端中输入任意的特殊符号; 缺点是你需要首先知道特殊符号所对应的十六进制表示，不过这一点，你可以通过 **[Unicode Table](http://www.tamasoft.co.jp/en/general-info/unicode.html)** 来查询，并记下常用特殊符号的十六进制表示来解决。            

------------------------------------

## 方法二          

使用该方法，你首先要启用Ubuntu中的 **[组合键](https://en.wikipedia.org/wiki/Compose_key)**, 操作方法如下:           

1. 打开Ubuntu的 **系统设置**              
2. 单击 **键盘**    
3. 在打开的窗口中单击 **快捷键**            
4. 在窗口左侧的一栏里单击 **打字**            
5. 在窗口右侧的一栏里单击 **组合键**, 之后按下你想要使用的键来作为组合键，在这里，我使用右 \<Alt> 键来作为组合键。                

在设置好组合键后，打开文件 **/usr/share/X11/locale/en_US.UTF-8/Compose**，其中部分内容如下:                 

```
...    
<Multi_key> <L> <minus>			: "£"	sterling
<Multi_key> <minus> <L>			: "£"	sterling
<Multi_key> <l> <equal>			: "£"	sterling
<Multi_key> <equal> <l>			: "£"	sterling
<Multi_key> <L> <equal>			: "£"	sterling
<Multi_key> <equal> <L>			: "£"	sterling
<Multi_key> <y> <minus>			: "¥"	yen
<Multi_key> <minus> <y>			: "¥"	yen
<Multi_key> <Y> <minus>			: "¥"	yen
<Multi_key> <minus> <Y>			: "¥"	yen
<Multi_key> <y> <equal>			: "¥"	yen
<Multi_key> <equal> <y>			: "¥"	yen
<Multi_key> <Y> <equal>			: "¥"	yen
<Multi_key> <equal> <Y>			: "¥"	yen
...
```

这个文件中的前三列表示了输入特殊字符所要按下的组合键，其中 <Multi_key> 就表示你刚刚设置的组合键，文件的第5列内容表示要输入的特殊符号, 你可以通过查询该文件来了解输入特殊符号所需要的按键。     

作为例子，假如我想要在终端输入 £ 符号，那么需要按下的键就是    

```
'右<Alt>键' + L + -        
```

**注意:** 由于我将右 \<Alt> 键设置为了组合键，因此，在上面的例子中我输入了右 <Alt> 键，如果你是设置的其他键来作为组合键的，你应该使用相应的键来替换上面的 '右 \<Alt> 键'.                

### 优缺点分析           

该方法的优点是在输入特殊符号时，速度可能会更加快；缺点是输入特殊符号时，需要记忆特定的按键，并且也只能输入提供的特殊符号，而不能输入任意的Unicode字符。           

------------------------------------

## 结束语

在很多时候，我们需要在文本中输入一些特殊符号，了解一些输入特殊符号的方法，会让我们的学习、工作更加便捷。本文介绍了两种在Ubuntu的终端中输入特殊符号的方法。我个人比较喜欢第一种方法，并且我也推荐你使用第一种方法，因为该方法使用起来更加便捷，并且可以输入任意的Unicode字符。              

最后，希望这篇文章可以帮助到你。               


<!--
Reference:         

在Linux终端插入特殊字符:            
https://superuser.com/questions/59418/how-to-type-special-characters-in-linux

在HTML文件中插入特殊字符:          
&#{number}

Enable Compose Key:
https://askubuntu.com/questions/452705/how-to-configure-compose-key-in-14-04

Unicode Table        
http://www.tamasoft.co.jp/en/general-info/unicode.html
-->           
