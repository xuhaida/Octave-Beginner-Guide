你经常需要执行一系列的命令。除了Octave命令行提示符后直接输入这些命令外，读者也可以将它们写至一个文本文件，然后在提示符里执行这个文件。这使你可以做一些必要的变动，然后直接运行这个文件而不需要费劲地重新一个命令接一个命令地反复输入。由一个或多个命令组成的文件被称为脚本文件，或者直接称作脚本。本章中读者会学会如何编写简单的脚本文件，以及带有复杂控制结构的脚本。

有时，保存你的工作成果是有用的。包括主要结果。本章中，我们也会教你如何做这些工作，如何将变量重新导入至Octave工作区（workspace）

总之，在本章里，读者会学到：

 - 什么是脚本，以及如何运行；
 - 如何使用if和switch来控制脚本中命令的运行；
 - 学会使用for，while，和do 语句
 - 控制异常的处理
 - 如何保存和导入你的工作成果
 - 有关将文本打印至屏幕和如何从用户得到输入；

**编写和运行你的第一个脚本**

我们会从非常简单的内容开始。在第3章里，我们讨论了如何从一个矩阵中得到最小值（命令行20-23)。让我们试着做同样的事情，不过这次我们使用脚本。

**实践时刻-制作你的第一个脚本**

 1. 启动Octave交互环境，打开编辑器：

        octave:1> edit

 2. 在编辑器里写入下面的命令，但是删除井号和代码行号。这些是用于参考用的：

    代码示例4.1

        A=rand(3,5);
        min(min(A));

 3. 将文件保存为script41.m(注意扩展名.m)至当前的目录或者任何Octave可以搜索到的目录。

 4. 现在可以运行脚本中的命令了，输入：

        octave：2>script41
        ans=
              0.1201

**发生了什么事**

在命令1中，我们打开了编辑器，然后写了两个Octave命令。命令2中，当我们要求Octave运行文本文件，它就运行了文件中的每一条命令。

在代码示例4.1中我们在第3行中没有加分号，命令返回了结果，ans显示。

文件扩展名.m对于Matlab的兼容性是必要的。但是，如果运行命令的话，并不是必要的。为了保证不管扩展名是什么都可以运行，读者可以使用source("filename"),这令filename用实际文件名来代替。

**改进脚本：input和disp**

与用户的交互具有可能性。这可以通过使用input和disp来实现。input使用下面命令调用：

        a=input(prompt string,"s")

这里，prompt string 是文本字符，而“s”是选项如果输入字符是必须的。下面是一些Octave命令的一些例子：

        octave: 3>a=input("Enter a number: ");        
        Enter a number: 42
        octave: 4>a
        a=
            42

注意，将一个变量作为输入参数，disp函数也有效，如果你输入变量名却没有分号，disp也可以应用于结构体和元胞数组。

我们可以在用户的脚本中使用input和disp函数进行交互。

**实战时刻--用户交互**

1. 打开一个新文件，将下面的命令写入至编辑器中：

        代码示例4.2
        nr= input（“Enter the number of rows in the matrix: ”);
        nc= input ("Enter the number of columns in the matrix: ")

        A=rand(nr,nc);

        minA=min(min(A));

        disp("The minimum of A is");
        disp(minA);

保存至script42.m。

2. 运行代码示例42，我们得到：

        octave:10> script42

**刚刚发生了什么**

代码示例4.2允许用户指定数组的大小（第1行和第2行）。就像代码示例4.1，脚本找到了数组的最小值。结果使用disp函数被打印。

**请刷新输出缓冲**

在一些系统里，你想打印文字需要缓冲。基本上，这意味着文字可以呆在一个队列等待被 刷新，可能会变成恼人的问题。为了避免该问题，你可以对缓冲刷新，在你提示用户输入时。这个通过命令fflush（stdout），stdout为输出缓冲（或者说流）。比如，味蕾保证input命令3之前stdout流会被刷新，我们使用：

        octave:11> fflush(stdout);
        octave:12> a=input("Enter a number: ");

**注释**

当你的脚本你变得越来越大，越来越复杂时，加入注释来解释命令的行为是很有用的。如果你或其他人将来需要使用和变更该脚本特别有用。使用井号#或者百分号%开始的每行代码会被解析器忽略，比如：

        octave:13> a
        octave:14> # a
        octave:15> % a

上面的命令中，你可以看到如果你省略了井号或者百分号，Octave会输出a的值（命令13中我们可以看到输出值42）。以#或者%开始的代码可以让Octave忽略改行代码。

让我们在代码示例4.2中加入一些注释，并且刷新流：

写注释时我使用了井号，于matlab不兼容，因为matlab使用百分号。

**长命令**

有时你需要写一个非常长的命令。比如，我们看到第3章，set函数可以调用许多输入参数。将一行命令打断成几行，你可以使用。比如，代码示例4.3可以分成2行：

        nr=input("Enter the number of rows ...
                  in the matrix: ")

或者：

        nr=input("Enter the number of rows \
                  in the matrix: ");

反斜号是Unix系统中表明续行操作。省略号操作符用于与Matlab的兼容性。

**工作区**

当你运行Octave命令提示中运行Octave脚本时，初始化后的变量会储存至当前的工作区列，指导缴入运行结束后都可以读取。让我们用一个例子加以说明：

        octave:16> clear
        octave:17> who
        octave:18> script41
        octave:19> who

跟踪你初始化后的变量是非常重要，包括脚本中初始化中的变量。假设我们忘记命令A初始化生成的A值，我们现在输入下面的命令：

        octave:20> A(:,1)=[0:10]

现在，Octave抱怨说维度不匹配，因为我们不能改变A中列的长度。如果A还没有被初始化，该命令会完成有效。为了避免这种情况，我经常调用clear，在脚本你的开始。这个命令清除所有你初始化的所有变量，这样做之前要小心。

**对于Gnu/Linux和MacOS X用户**

在Gnu/Linux和MacOS X，你可以调用Octave脚本，直接从shell中直接运行，因此你不需要开启Octave的交换环境。为了这样做，我们只需要：

1. 找出Octave的安装位置，典型地输入：/usr/bin/octave

2. 在脚本script43.m中加入下面这一行：

        #! /usr/bin/octave -qt

3. 保存文件，退出Octave。

4. 确认你在文件保存的目录，在shell的提示符中，输入：

        $ chmod u+x script43.m
   来允许文件可以运行。

5. 现在输入：

        $./script43.m

   你会看到脚本如何运行，就像Octave提示中看到的。

**语句**

在前面部分，我们学会如何编写一个简单的脚本，我们会看到脚本中只是一系列的命令。在本节中，你会学习如何使用语句来控制脚本的行为。这会使你编写脚本执行许多和复杂的任务。

**质数问题**

我们会讨论多种语句：if,for ,while等待，来求解一个整数是否为质数。正如你所知，质数x为自然数只有两个除数1和自己。除数y是自然数y，是x/y的余数，从质数的定义中，我们可以写，然后我们不需要检查任意一个偶数，该算法极其粗糙，还有更多有效的方法来判定一个数是否为质数。

**决策-if语句**

从程序流程图中，可以看到我们决定一个数是否质数，通过该数是否可以。为了在Octave中这样做，你可以使用if和elseif语句，通用语法为：

        if condition 1
          do somethin (body)
        elseif condition 2
          do something else (body)
        ...
        else
          do somthing if no conditiongs are met (body)
        endif

如果条件1为真（非零值），if语句会运行。如果条件2为假，条件2为真，elseif部分会运行。elseif和else语句是可选的。让我们用一个小代码片段加以说明，来检查流程图中的前2个条件是否复核：

        if (x<2)
          disp("x not a prime")
        elseif( rem (x,2)==0)
          disp("x not a prime")
        else
          disp("x could be a prime number")
        endif

rem函数返回x/2之后的余数，如果第1个语句体运行，意味着的比较操作x<2的值为真，elseif和else中的语句就不会求值。Octave解析器简单地跳到endif之后的语句。同业，如果rem(x,2)==0为真，else语句就不会运行。该语句当且仅当if和elseif均为非真时运行。

你也可以将这些语句至于Octave命令提示符中。在这里做一些简单的测试是个不错的主意。来实际看看上面的代码片段发生了什么，我们使用：

        octave:21> x=9
        octave:22> if(x<2)
        >disp("x not a prime");
        >elseif( rem (x,2)==0)
        >disp("x not a prime");
        >else
        >disp("x could be a prime number");
        >endif

是的，它不是质数。如果你编码有错误或者打字有错误，Octave会告诉你。

**插曲:布尔操作符**

在上面的代码片段里，我们编写2行相同的代码，每行代码容易出错，如果可能，应尽量避免重复代码，除非有特别的理由。Octave给你提供一整套布尔操作（你在本书第3章里已经学过），我们可以将多个比较操作包含于于单个语句中，从而可以避免重复。

**按元素布尔操作**

按元素布尔操作有3个：&,|和！。也许最简单对他们进行讨论它们使用方法的是通过Octave命令提示中输入2个例子。

命令23很短。命令24中，我们使用&操作符在2个布尔之间。通过比较操作符A== eye（2）(左边)和B==eye(2)(右边)。回想一下第2章里的比较操作符，得到布尔类型。现在，左边布尔值为矩阵，其中所有元素为真。同样，右边布尔也为举证，其中所有元素为假，除了第一列第一行为真。&操作符简单地求值。如此，布尔操作符为真，下面的图中说明，注意，如果两个值均为假，运算结果为假。

与&操作符不一样，|操作符只有当左边布尔值为真或者右边布尔值为真时，结果才为真。比如：

        octave:25> A==eye(2) | B==eye(2)

因为A与eye(2)相同。

布尔操作符！取否操作。意思如果布尔变量a为真，！a为假。比如，因为A为单位矩阵，可以视为布尔变量，其中对角元素为真，而非对角元素为假。变量A取否：

        octave:26> !A

**短路布尔操作符**

&和|操作符，意思是操作符求值对所有元素成对。有时，你也许想知道，A是否与eye（2）相同，B 是否与eye（2）相同，而不是按单个元素进行。为这个目的，我们可以使用短路布尔操作符&&：

        octave:27> A==eye(2) && B==eye(2)

结果告诉我们不成立。同样，我们也可以使用短路操作符||：

        octave:28> A==eye(2) || B==eye(2)

该结果是因为A初始化时被设置成eye（2）

下面列表中总结了布尔操作符&, &&, |和||的输出结果：

**if语句中使用布尔操作符**

除了使用if和elseif语句结构外，我们现在将通用的条件运用于if语句：

        if (x<2 | rem(x,2)==0)
          disp("x is not a prime");
        else
          disp("x could be a prime")
        endif

如果x为，比如说，2，然后比较操作符x<2取值为假，rem（x，2）==0 取值为真，所以|操作符运算结果为真根据表中，符合if语句中的条件。另外，如果x为9，然后比较操作x<2和rem（x,2)= =0取值为假，if语句体不会运行。在这个例子中，你可以使用短路操作符||，因为x为简单的标量变量。

**套嵌语句**

与其他编程语言一样，你可以将if语句置于if，elseif和else语句里。比如：

      if (x<2 | rem(x,2)==0)
        disp("x is not a prime");
      else
        if (x>3 & rem(x,3)==0)
          isp("x not a prime")
        else
          disp("x could be a prime")
        endif
      endif

**switch语句**

一些程序员会选择使用switch语句结构，而不是if语句。这样做是可以的，并且可以明显改进代码的可读性。通用的语法为：

        switch option
        case option
          do something (body)
        case option
          do something (body)
        ...
        otherwise
          do something default (body)
        endswitch

通过重写上面代码我们来解释switch语句：

        switch(x2 | rem(x,2)==0)
          case 1
            disp("x not a prime")
          otherwise
            disp("x could be a prime")
        endswitch

这个程序流程是很清晰的。

**循环语句**

从上面的程序流程图，可以看到我们需要计算x/y的余数，对于所有小于x的y值，因此如果x比较大，我们需要多次调用rem函数。Octave中，我们可以使用for，while和do来实现。

**for语句**

语法很简单：

        for condition
          do something (body);
        endfor

for语句于对应的endfor构成了for循环语句。只有condition得到满足，for循环就会运行。我们看一段代码：



Octave首次运行for循环语句，y值被设定为3，第2次，y值设定为4，等等。我们说，y被增加1.当y值等于x值时，条件已经不能满足，因为y值从3到x-1，正如第1行代码，循环停止运行。

从质数的定义，我们知道我们不需要计算所有偶数的余数。我们可以忽略偶数,将y赋值为3，5，7等等。将y=3然后，每次都加上2，将第1行的代码替换成下面的代码：

        for y=3:2:x-1

这样，我们的计算量减了一半。

上面的代码里还计算所有y<x的余数，甚至我们知道它为0.

**while和do语句**

for可替换成while和do语句。while语句语法为：

        while condition
          do something(body)
        endwhile

上面代码片段可以直接重新实现：

        y=3;
        while y<x
          if(rem(x,y)==0)
            disp("x is not a prime");
            break;
          endif
          y=y+2;
        endwhile

在第1行代码，我们初始化y为3，在我们进入while语句之前。代码第3-6行，检查x是否为质数，第7行代码中，y被递增2.

注意，break命令也可以用于打破while循环。

我们也许也想使用break关键字，比如

        y=3;
        while y<x
          if(rem(x,y)！=0)
            continue;
          else
            disp("x is not a prime");
          endif
          y=y+2;
        endwhile

但是，这会导致死循环，应为

你可以
