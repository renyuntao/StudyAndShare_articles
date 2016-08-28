# 如何在Shell Script中判断输入的用户密码是否正确?           
<!--
2016-08-28
--><br />

## 验证密码输入正确与否的意义           

在学一个新东西的时候，首先要知道的是，所学的东西是干什么的，它的作用是什么，以及可以用在哪里。明白了这些，在学习时才会有目标，有动力; 否则，就只是在盲目地学习，这样会感到十分枯燥，而且学过之后也很容易忘记。因此，在讲解下面的内容之前，首先介绍一下在哪里会用到验证密码的输入是否正确，以及它的作用及意义。                 
             
在写Shell脚本时，用到验证密码输入正确与否的地方，通常是用在有相关的权限限制的地方。例如某些操作只能由特定的用户来操作，这样，在程序中就会要求输入该用户的密码，密码输入正确以后，相应的操作才会被执行。此时，就需要在Shell Script中验证输入的密码是否与相应的用户的密码是否相匹配了。       

总之，验证输入的密码正确与否，是为了设置相应的权限，使得一些操作只能有特定的用户来执行; 它的作用或意义就是增加隐私性和系统的安全性。     

上面简单地介绍了验证密码输入的用武之地，以及相应的作用和意义，下面来解释如何在Shell Script中验证输入的密码是否与相应的用户的密码相匹配。      

-----------------------------------

## 借助 sudo

假如要判断的用户可以执行 `sudo` 来提升自己的权限，那么我们可以在Shell Script中借助 `sudo` 来判断输入的密码是否与该用户的密码匹配，具体操作如下:   

```bash
#!/usr/bin/env bash

read -s -p "Input password" password
echo "$password" | sudo -S ls &> /dev/null

# 密码正确
if [ "$?" == "0" ]
then
	echo "The password is correct"
	# do something
	# ....

# 密码错误
else
	echo "The password is incorrect"
	# do something else
	# ...
fi
```

如果上面程序的输出结果是           

```bash
The password is correct
```

则表明输入的密码是正确的。                   

如果输出结果是              

```bash
The password is incorrect
```

则表明输入的密码是错误的。               

-----------------------------------

## 借助 su

假如要判断的用户不能执行 `sudo` 来获取root权限，或者是要判断的用户本身就是root用户，那么就不能够使用上面所说的方法了，此时，我们可以借助 `su` 这个工具来判断输入的密码是否与要判断的用户的密码相匹配。                   

推荐阅读: [Linux:普通用户如何获取root权限?](how_normal_user_get_root.html)          
         
这里假设要判断的用户的用户名为 `techforgeek`, 判断的方法如下:             

```bash
#!/usr/bin/env bash

echo -n "Input password: "
su - techforgeek -c exit &> /dev/null

# 密码正确
if [ "$?" == "0" ]
then
	echo -e "\nThe password is correct"
	# do something
	# ...

# 密码错误
else
	echo -e "\nThe password is incorrect"
	# do something else
	# ...
fi
```

**注意, 你在运行上面的程序的时候，需要将程序中的 `techforgeek` 替换为你想要判断的用户的用户名。**                                     

如果上面程序的输出结果是           

```bash
The password is correct
```

则表明输入的密码是正确的。                   

如果输出结果是              

```bash
The password is incorrect
```

则表明输入的密码是错误的。               

-----------------------------------

## 结束语          

一名 **Linux Power User** 总是喜欢写一些Shell脚本来管理自己的主机，从而让自己的生活更加简单。在写Shell脚本时，有时会涉及到一些敏感操作，我们希望这个操作只有我们自己才能够执行，这时我们可以在Shell脚本中增加密码验证的功能。这篇文章中介绍了两种用于密码验证的方法, 如果你要在你的Shell脚本中增加密码验证的功能，希望这篇文章可以帮助到你。                 
