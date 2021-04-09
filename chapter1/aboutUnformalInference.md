# 17和1.8在讲什么？

**形式推理和非形式推理**

 看一个例子：

> 证明： 奇数的平方也是奇数

## 1 形式证明

首先问题被翻译为谓词逻辑公式如下：^2
$$
\forall x \left( odd(x) \rightarrow odd(squre(x)) \right) \tag{1}
$$
备注：

- 谓词$odd(x)$表示是$x$奇数
- 函数是普通函数（区别于命题函数），它的定义域同命题函数一样，但它的值域不是$ \{0,1\} $，而是与它的定义域一样。

证明过程如下：

(1)  $ \forall x (odd(x) \leftrightarrow equal(x, 2 k + 1)$           P规则    注意： 这里的 equal(x,y)是一个谓词：x与y相等。

(2) $odd(a) \leftrightarrow eauql(a,2k + 1)$                    UI规则  from(1)

(3) $\forall x (equal(x,y) \leftrightarrow equal(x^2,y^2))$          定理，恒等定律

(4)  $equal (a,2k + 1) \leftrightarrow equal(a^2,(2 k + 1)^2) $   UI规则 from(3)

(5 ) $equal(a^2,(2 k + 1)^2) \leftrightarrow equal(a^2, 4 k^2 + 4k + 1)$  定理$equal((a+b)^2,(a^2+2ab+b))$

(6) $equal(a^2, 4 k^2 + 4k + 1) \leftrightarrow equal(a^2)$  定理$equal((a+b)^2,(a^2+2ab+b))$

(7) $\forall x \forall y \forall z（（equal(x,y) \wedge equal(y,z) ) \rightarrow equal(x,z)）$ 公理 相等的传递性

