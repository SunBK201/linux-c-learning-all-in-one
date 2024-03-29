# 4. 第一个程序

通常一本教编程的书中第一个例子都是打印“Hello, World.”，这个传统源自 [\[K&R\]](../../../appendix/reference.md#standardcstandardca-referencepj-plauger-he-jim-brodie)，用C语言写这个程序可以这样写：

```c
#include <stdio.h>

/* main: generate some simple output */

int main(void)
{
    printf("Hello, world.\n");
    return 0;
}
```

将这个程序保存成main.c，然后编译执行：

```text
$ gcc main.c
$ ./a.out
Hello, world.
```

gcc是Linux平台的C编译器，编译后在当前目录下生成可执行文件a.out，直接在命令行输入这个可执行文件的路径就可以执行它。如果不想把文件名叫a.out，可以用gcc的-o参数自己指定文件名：

```text
$ gcc main.c -o main $ ./main Hello, world.
```

虽然这只是一个很小的程序，但我们目前暂时还不具备相关的知识来完全理解这个程序，比如程序的第一行，还有程序主体的 `int main(void){...return 0;}` 结构，这些部分我们暂时不详细解释，读者现在只需要把它们看成是每个程序按惯例必须要写的部分（Boilerplate） 。但要注意 `main` 是一个特殊的名字，C程序总是从 `main` 里面的第一条语句开始执行的，在这个程序中是指 `printf` 这条语句。

第3行的 `/* ... */` 结构是一个注释（Comment） ，其中可以写一些描述性的话，解释这段程序在做什么。注释只是写给程序员看的，编译器会忽略从 `/*` 到 `*/` 的所有字符，所以写注释没有语法规则，爱怎么写就怎么写，并且不管写多少都不会被编译进可执行文件中。

`printf` 语句的作用是把消息打印到屏幕。注意语句的末尾以 `;` 分号（Semicolon） 结束，下一条语句 `return 0;` 也是如此。

C语言用 `{}` 花括号（Brace或Curly Brace） 把语法结构分成组，在上面的程序中\`\`printf\`\` 和 `return` 语句套在 `main` 的 `{}` 括号中，表示它们属于 `main` 的定义之中。我们看到这两句相比 `main` 那一行都缩进（Indent） 了一些，在代码中可以用若干个空格（Blank） 和Tab字符来缩进，缩进不是必须的，但这样使我们更容易看出这两行是属于 `main` 的定义之中的，要写出漂亮的程序必须有整齐的缩进，第 1 节 “缩进和空白”将介绍推荐的缩进写法。

正如前面所说，编译器对于语法错误是毫不留情的，如果你的程序有一点拼写错误，例如第一行写成了 `stdoi.h` ，在编译时会得到错误提示：

```bash
$ gcc .\hello.c
.\hello.c:1:10: fatal error: stdoi.h: No such file or directory
#include <stdoi.h>
        ^~~~~~~~~
compilation terminated.
```

这个错误提示非常紧凑，初学者往往不容易看明白出了什么错误，即使知道这个错误提示说的是第1行有错误，很多初学者对照着书看好几遍也看不出自己这一行哪里有错误，因为他们对符号和拼写不敏感（尤其是英文较差的初学者），他们还不知道这些符号是什么意思又如何能记住正确的拼写？对于初学者来说，最想看到的错误提示其实是这样的：“在 `main.c` 程序第1行的第19列，您试图包含一个叫做 `stdoi.h` 的文件，可惜我没有找到这个文件，但我却找到了一个叫做 `stdio.h` 的文件，我猜这个才是您想要的，对吗？”可惜没有任何编译器会友善到这个程度，大多数时候你所得到的错误提示并不能直接指出谁是犯人，而只是一个线索，你需要根据这个线索做一些侦探和推理。

有些时候编译器的提示信息不是 **error** 而是 **warning** ，例如把上例中的 `printf("Hello, world.\n");` 改成 `printf(1);` 然后编译运行：

```bash
$ gcc main.c
main.c: In function ‘main’:
main.c:7: warning: passing argument 1 of ‘printf’ makes pointer from integer without a cast
$ ./a.out
Segmentation fault
```

这个警告信息是说类型不匹配，但勉强还能配得上。警告信息不是致命错误，编译仍然可以继续，如果整个编译过程只有警告信息而没有错误信息，仍然可以生成可执行文件。但是，警告信息也是不容忽视的。出警告信息说明你的程序写得不够规范，可能有Bug，虽然能编译生成可执行文件，但程序的运行结果往往是不正确的，例如上面的程序运行时出了一个段错误，这属于运行时错误。各种警告信息的严重程度不同，像上面这种警告几乎一定表明程序中有Bug，而另外一些警告只表明程序写得不够规范，一般还是能正确运行的，有些不重要的警告信息 gcc 默认是不提示的，但这些警告信息也有可能表明程序中有Bug。一个好的习惯是打开 gcc 的 `-Wall` 选项，也就是让 gcc 提示所有的警告信息，不管是严重的还是不严重的，然后把这些问题从代码中全部消灭。比如把上例中的 `printf("Hello, world.\n");` 改成 `printf(0);` 然后编译运行：

```bash
$ gcc main.c
$ ./a.out
```

编译既不报错也不报警告，一切正常，但是运行程序什么也不打印。如果打开 `-Wall` 选项编译就会报警告了：

```bash
$ gcc -Wall main.c
main.c: In function ‘main’:
main.c:7: warning: null argument where non-null required (argument 1)
```

如果 `printf` 中的 `0` 是你不小心写上去的（例如错误地使用了编辑器的查找替换功能），这个警告就能帮助你发现错误。虽然本书的命令行为了突出重点通常省略 `-Wall` 选项，但是强烈建议你写每一个编译命令时都加上 `-Wall` 选项。

