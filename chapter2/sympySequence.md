# 序列（Sequence）的sympy操作

[**hawksoft**](https;//gitee.com/xingyongkang)

[toc]

## 1. 基本概念


序列（sequence），定义为一系列有序对象。这些对象称为序列的项（term）。一个序列包含项的数目可以是有限的，也可以无限，但必须是可数的。因此，按照顺序，可以给序列的每一项给一个自然数的编号，称为索引。

你可以把序列理解为python中list。

序列有两种表示方式：
1. 罗列法。如$a_0,a_1,a_2,\cdots a_n$
2. 生成公式法。序列可以看成是定义在自然数集合$N$到序列项的集合上的双射函数，因此可以通过函数来描述序列，即：$a_n = f(n) $ 。 $f(n)$称为序列的生成公式。

生成公式是表示和理解序列的基本方法。

另外要注意：两个序列即使生成公式相同，若其定义域不同（尽管都是$N$的子集），仍然是不同的序列。序列的定义域称为序列的区间（interval）。

## 2 建立序列

记住：定义一个序列，既要定义其生成公式，还要定义它的区间。

在sympy中，类SeqFormula用来通过公式法生成一个序列对象。序列对象的常见属性和方法很容易理解，其中的gen和interval属性，分别对应序列的生成公式和区间。


```python
#这段代码只是为了美化输出，定义了一个输出latex公式字符串的函数lprint
def lprint(content,First = False,Last = False):
    if First:
        print('上面代码的输出如下（每次调用输出一行）：\n\n')
        print('$$')
        print('\\begin{align}')
    s = latex(content)
    #print('{} {} {}'.format('&&',s,'\\\\'))
    print('{} {} {} {}'.format('&',s,'&','\\\\'))
    if Last:
        print('\\end{align}')
        print('$$')
```


```python
from sympy import SeqFormula,pprint,oo,latex
from sympy.abc import n,a,b,r
s1 = SeqFormula(a*r**n,(n,0,8)) #创建一个下标从0到8的几何序列
lprint(s1,First=True)                      #显示序列
lprint(s1.start)                #序列的开始和结束下标
lprint(s1.stop)
lprint(s1.length)                #序列长度  
lprint(s1.interval)

s2 = SeqFormula(a*r**n,(n,3,5)) #创建一个下标从3到5的几何序列
lprint(latex(s2))                      #显示序列
lprint(s2.start)                #序列的开始和结束下标
lprint(s2.stop)
lprint(s2.length)                #序列长度  
lprint(s2.interval)

lprint(s1.gen)                  #序列的生成公式
lprint(s2.gen,Last=True)                  #序列的生成公式
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& \left[a, a r, a r^{2}, a r^{3}, \ldots\right] & \\
& 0 & \\
& 8 & \\
& 9 & \\
& \left[0, 8\right] & \\
& \left[a r^{3}, a r^{4}, a r^{5}\right] & \\
& 3 & \\
& 5 & \\
& 3 & \\
& \left[3, 5\right] & \\
& a r^{n} & \\
& a r^{n} & \\
\end{align}
$$

有些具有周期性的序列，可以用类SeqPer来建立。


```python
from sympy import SeqPer
s2 = SeqPer((1,2,3),(n,0,5))
lprint(s2[:],First=True)
lprint(s2.period)
lprint(s2.periodical)
lprint(s2.start)                #序列的开始和结束下标
lprint(s2.stop)
lprint(s2.length)                #序列长度  
lprint(s2.interval,Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& \left[ 1, \  2, \  3, \  1, \  2, \  3\right] & \\
& 3 & \\
& \left( 1, \  2, \  3\right) & \\
& 0 & \\
& 5 & \\
& 6 & \\
& \left[0, 5\right] & \\
\end{align}
$$

## 3  序列运算

序列运算，是基于对应项进行运算。（sympy在实现中，分别定义了对应的符号类，如：SeqAdd，SeqMul等，现在可以不理解）。


```python
from sympy import SeqFormula,oo
from sympy.abc import m,n,a,b,r
s1 = SeqFormula(a*r**n,(n,0,8)) 
lprint(s1,First=True)                      
s2 = SeqFormula(b*r**m,(m,0,8)) 
lprint(s2)                      
lprint(s1 + s2)
lprint(s1 - s2)
lprint(s1 * s2)
lprint(s1 * s2)
lprint(s1.coeff_mul(2),Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& \left[a, a r, a r^{2}, a r^{3}, \ldots\right] & \\
& \left[b, b r, b r^{2}, b r^{3}, \ldots\right] & \\
& \left[a + b, a r + b r, a r^{2} + b r^{2}, a r^{3} + b r^{3}, \ldots\right] & \\
& \left[a - b, a r - b r, a r^{2} - b r^{2}, a r^{3} - b r^{3}, \ldots\right] & \\
& \left[a b, a b r^{2}, a b r^{4}, a b r^{6}, \ldots\right] & \\
& \left[a b, a b r^{2}, a b r^{4}, a b r^{6}, \ldots\right] & \\
& \left[2 a, 2 a r, 2 a r^{2}, 2 a r^{3}, \ldots\right] & \\
\end{align}
$$

## 4 序列项连加

序列求和，是通过函数$summation()$实现的。该函数需要两个参数，第一个是序列的生成公式，第二个是求和的区间。


```python
from sympy import SeqFormula,oo,summation,factor,expand
from sympy.abc import m,n,a,b,r,k
s1 = SeqFormula(a*r**n,(n,0,8)) 
s11 = summation(s1.gen,(n,0,8))
lprint(s11,First=True)
lprint(factor(s11))
s12 = s11.subs([(a,1),(r,2)])
lprint(s12,Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& a \left(r^{8} + r^{7} + r^{6} + r^{5} + r^{4} + r^{3} + r^{2} + r + 1\right) & \\
& a \left(r^{2} + r + 1\right) \left(r^{6} + r^{3} + 1\right) & \\
& 511 & \\
\end{align}
$$

注意符号运算与数值运算的区别。只有把$a$和$r$都指定了具体的数值，才能计算出数值结果来，其它都是符号结果。在sympy中，有时甚至所有的符号都指定了数值，仍然默认不会输出数值结果。如阶乘计算等。这可以执行一个方法$doit()$，才会输出数值结果。


```python
from sympy import SeqFormula,oo,summation,factor,expand
from sympy.abc import m,n,a,b,r,k
s1 = SeqFormula(a*r**n,(n,0,8)) 
s11 = summation(s1.gen,(n,0,m))
lprint(s11,First=True)
s12 = s11.subs([(a,1),(r,2)])
lprint(s12)
lprint(s12.subs(m,10),Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& a \left(\begin{cases} m + 1 & \text{for}\: r = 1 \\\frac{1 - r^{m + 1}}{1 - r} & \text{otherwise} \end{cases}\right) & \\
& 2^{m + 1} - 1 & \\
& 2047 & \\
\end{align}
$$

等差序列求和：


```python
from sympy import *
from sympy.abc import a,r,k,n,d
s1 = SeqFormula(a*d*k,(k,0,8))
s11 = summation(s1.gen,(k,0,n))
lprint(s11,First=True)
s2 = s11.subs([(a,1),(d,1)])
lprint(s2)
lprint(factor(s2),Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& a d \left(\frac{n^{2}}{2} + \frac{n}{2}\right) & \\
& \frac{n^{2}}{2} + \frac{n}{2} & \\
& \frac{n \left(n + 1\right)}{2} & \\
\end{align}
$$

阶乘序列求和：


```python
from sympy import *
from sympy.abc import a,r,k,n,d
s1 = SeqFormula(a*factorial(k),(k,0,9))
s11 = summation(s1.gen,(k,0,n))
lprint(s11,First=True)
s2 = s11.subs(a,1)
lprint((s2.subs(n,10)).doit(),Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& \sum_{k=0}^{n} a k! & \\
& 4037914 & \\
\end{align}
$$

## 5 序列项连乘

序列连乘，是通过函数$product()$实现的。与序列连加一样，该函数也需要两个参数，第一个是序列的生成公式，第二个是连乘的区间。


```python
from sympy import SeqFormula,oo,product,factor,expand
from sympy.abc import m,n,a,b,r,k
s1 = SeqFormula(a*r**n,(n,0,8)) 
s11 =product(s1.gen,(n,0,m))
lprint(s11,First=True)
s12 = s11.subs([(a,1),(r,2),(m,3)])
lprint(s12,Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& a^{m + 1} r^{\frac{m^{2}}{2} + \frac{m}{2}} & \\
& 64 & \\
\end{align}
$$

## 6 其它


这里强调一下，不要把级数（series）与序列混淆。在sympy中，级数特指函数的泰勒级数，如下面的代码：


```python
from sympy import series,cos,summation
from sympy.abc import x,y,z
s1 = series(cos(x),x,0,3)
lprint(s1,First=True)
s1 = series(x**2,x,y,3)
lprint(s1,Last=True)
```

上面代码的输出如下（每次调用输出一行）：


$$
\begin{align}
& 1 - \frac{x^{2}}{2} + O\left(x^{3}\right) & \\
& y^{2} + 2 y \left(x - y\right) + \left(x - y\right)^{2} & \\
\end{align}
$$
