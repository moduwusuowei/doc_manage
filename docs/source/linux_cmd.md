# Linux cmd



## ls

英文原意：list directory contents

功能描述：用来显示指定目录内的文件列表，可通过选项控制显示内容的详细程度和颜色高亮等

### 案例展示

```
axera@axera-OptiPlex-7080:~/wusong$ ls
1.sh  1.txt

```



**1. [-a] 选项**

长短格式对照：[-a] == [--all]

显示所有文件，包含以 . 开头的隐藏文件以及特殊目录

```

axera@axera-OptiPlex-7080:~/wusong$ ls -a
.  ..  1.sh  1.txt

```



**2.** **[-A]** **选项**

长短格式对照：[-A] == [--almost-all]

显示所有文件，包含以 . 开头的隐藏文件，但不显示特殊文件 . 和 ..

```

axera@axera-OptiPlex-7080:~/wusong$ ls -A
1.sh  1.txt

```



注意：ls -a 命令下面显示的 . 和 .. 两个特殊文件功能分别是

. #代表当前所在目录

.. #代表当前所在目录的父目录，即上一级目录

**3.** **[-l]** **选项**

长短格式对照：[-l] == [--format=long]

用长格式显示当前目录下文件的详细信息

```

axera@axera-OptiPlex-7080:~/wusong$ ls -l
total 0
-rw-rw-r-- 1 axera axera 0 11月 17 13:48 1.sh
-rw-rw-r-- 1 axera axera 0 11月 17 13:48 1.txt

```

显示内容中总共分为七列信息，分别是：

*第一列：*用来表示文件类型和文件权限

*第二列：*意为引用计数

普通文件的引用计数大于1时，代表该文件存在硬链接

目录文件的引用计数至少是2，代表目录内存在几个子目录（.和..特殊目录也是目录）

*第三列：*文件所有者的权限（属主权限）

*第四列：*文件所属组的权限（属组权限）

*第五列：*文件大小，默认以字节为单位显示，可以结合 -h 选项用较合适的单位显示

*第六列：*文件创建时间或者最近一次访问时间，时间比较近时显示顺序为{月 日 时间}，时间较远时，则仅显示年份

*第七列：*文件名

**4. [-d] 选项**

长短格式对照：[-d] == [--directory]

显示目录文件本身的信息，不在显示目录内的文件列表，一般结合-l使用

```
axera@axera-OptiPlex-7080:~/wusong$ ls
1.sh  1.txt  aa
axera@axera-OptiPlex-7080:~/wusong$ ls -d aa
aa
```

**5. [-h] 选项**

长短格式对照：[-h] == [--human-readable]

在显示文件详细信息时，使用 -h 可以让文件大小按照适合人类读取习惯的方式显示{即合理的单位显示文件大小}

```
axera@axera-OptiPlex-7080:~/wusong$ ls -h
1.sh  1.txt  aa
axera@axera-OptiPlex-7080:~/wusong$ ls -lh
total 4.0K
-rw-rw-r-- 1 axera axera    0 11月 17 13:48 1.sh
-rw-rw-r-- 1 axera axera    0 11月 17 13:48 1.txt
drwxrwxr-x 2 axera axera 4.0K 11月 17 13:50 aa

```

**6. [-i] 选项**

长短格式对照：[-i] == [--inode]

显示文件时，同时显示文件的 索引节点号（inode号）

```
axera@axera-OptiPlex-7080:~/wusong$ ls -i
218628183 1.sh  218628185 1.txt  218628186 aa

```

每个文件前边的数字即为文件的索引节点号（inode号），每一个 inode号代表一个文件

**7. [-s] 选项**

长短格式对照：[-s] == [--size]

显示每个文件占用的硬盘空间大小

```
axera@axera-OptiPlex-7080:~/wusong$ ls -i
218628183 1.sh  218628188 1.txt  218628186 aa
axera@axera-OptiPlex-7080:~/wusong$ ls -lsh
total 12K
   0 -rw-rw-r-- 1 axera axera    0 11月 17 13:48 1.sh
8.0K -rw-rw-r-- 1 axera axera 5.9K 11月 17 13:53 1.txt
4.0K drwxrwxr-x 2 axera axera 4.0K 11月 17 13:50 aa

```

由于 Linux 系统中绝大多数分区的 data block 都是 4k ，而且 data block 块具有独占性，导致一个文件的大小和改文件实际占用的硬盘是有区别的。

**8. [-F] 选项**

长短格式对照：[-F] == [--classify]

显示文件列表时，为每一个特殊文件在文件名结尾处追加一个符号，用来表示具体某种文件类型。

```
axera@axera-OptiPlex-7080:~/wusong$ ls -F
1.sh  1.txt  aa/
```

\* 代表具有可执行权限的普通文件

/ 代表目录文件

@ 代表符号链接文件（软链接）

| 代表管道符文件

= 代表socket套接字文件

啥也没标记代表普通文件

**9. [--color] 选项**

长短格式对照：[--color] == [无]

在终端上显示文件时，为不同类型文件附着不同的颜色



蓝色：目录文件

红色：压缩包文件等

天蓝：符号链接文件

可以人为控制显示结果中的颜色

--color=never 表示输出结果时没有颜色
--color=auto 表示按照文件类型自动显示颜色
--color=always 表示输出内容始终有颜色（多数情况与auto相同）

**10. ls 命令的相关别名**

```
axera@axera-OptiPlex-7080:~/wusong$ alias | grep -i ls
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -alF'
alias ls='ls --color=auto'
axera@axera-OptiPlex-7080:~/wusong$ la
1.sh  1.txt  aa

```



l. 代表显示目录下所有的隐藏文件

ll 代表显示目录下文件时，使用长格式显示文件详情

ls 代表显示目录下文件时，为不同类型的文件附着不同的颜色



**11. ls 命令结合通配符实现包含匹配显示**

```

axera@axera-OptiPlex-7080:~/wusong$ ls 1*
1.sh  1.txt
axera@axera-OptiPlex-7080:~/wusong$ ls *.txt
1.txt

```

**见通配符含义：**

\* 匹配任意长度的任意字符（包含空）

? 匹配长度为一的任意字符（即匹配一个任意字符）

[] 匹配括号内的任意一个字符，不连续的的字符串可以写在一起，连续的字符串需要用 - 作为连接符，比如 [a-f] 代表匹配 a 到 f 之间的任意一个字符。

### 总结

ls 命令的大部分选项在混合使用时是没有先后排列顺序要求的，但如果选项后面可以跟其他参数时，选项要单独声明，例如：ls --color=auto

还可以使用别名完成命令和选项的合体，形成新的别名命令，减少使用难度，提高使用效率。









## 文件写

使用循环结构和重定向操作符来实现向一个文件循环写入1至20的数字。以下是一个示例的Shell脚本代码：

```
#!/bin/bash

file="output.txt"

for ((i=1; i<=20; i++))
do
    echo $i >> $file
done
```



在这个示例中，我们使用了一个for循环来遍历1至20的数字。在每次循环中，我们使用echo命令将当前数字写入到指定的文件output.txt中，使用重定向操作符>>来实现追加写入。

你可以将上述代码保存为一个Shell脚本文件（例如write_numbers.sh），然后在终端中运行该脚本，即可将1至20的数字循环写入到output.txt文件中。

请注意，如果output.txt文件不存在，上述代码会自动创建该文件。如果文件已存在，上述代码会将数字追加写入到文件的末尾。如果你希望每次运行脚本时都重新创建文件并写入数字，可以在循环之前添加一行代码，使用重定向操作符>覆盖写入文件：

```
#!/bin/bash

file="output.txt"

> $file  # 清空文件内容

for ((i=1; i<=20; i++))
do
    echo $i >> $file
done

```



这样，在每次运行脚本时，会先清空文件内容，然后再循环写入1至20的数字。

希望对你有所帮助！如有任何进一步的问题，请随时提问。

## head&tail

查看文件前10行

```
 head -n 10 output.txt
```

查看文件后10行

```
 tail -n 10 output.txt
```

从10行开始，显示20行，即显示10-19行

```
 cat filename | tail -n +10 | head -n 20
```

显示100行到300行

```
 cat filename | head -n 300 | tail -n +100
```

分解

```
 tail -n 500 #显示最后500行
 tail -n +1000 #从1000行开始显示，显示1000行以后的
 head -n 1000 #显示前面1000行
```

## sed

查看文件100行到300行

```
 sed -n '5, 10p' filename
```