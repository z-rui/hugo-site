---
date: 2015-09-01
title: "和最大公约数有关的一个证明"
---

1. 已知\\(a>1\\), \\(m > n > 0\\)，求证\\(\gcd(a^m-1, a^n-1) = a^{\gcd(m,n)} - 1\\)；
2. 已知\\(a>b\\), \\(\gcd(a, b)=1\\)，求证\\(\gcd(a^m-b^m, a^n-b^n) = a^{\gcd(m,n)} - b^{\gcd(m,n)}\\)。

<!--more-->

仅就第2个命题给出证明，如下。

设\\(m = pg, n = qg\\)，其中\\(g = \gcd(m,n)\\)。易知\\(p\\)和\\(q\\)互质。设\\(x=a^g, y=b^g\\)，则显然\\(x\\)和\\(y\\)互质，且原命题的结论转化为

\\[\gcd(x^p-y^p, x^q-y^q) = x-y\\]

由等比级数公式
\\[1+r+r^2+\cdots+r^N = {1-r^{N+1}\over 1-r}\\]
可知
\\[r^{N+1} - 1= (r-1)(r^N+r^{N-1}+\cdots+1)\\]

若取\\(r = x/y\\), \\(N=p-1\\)，然后两边同乘以\\(y^p\\)，得
\\[x^p - y^p = (x-y)(x^{p-1}+x^{p-2}y+\cdots+y^{p-1})\\]

同理
\\[x^q - y^q = (x-y)(x^{q-1}+x^{q-2}y+\cdots+y^{q-1})\\]

由此可以看出，\\(x^p - y^p\\)和\\(x^q - y^q\\)具有公因子\\(x-y\\)。下面证明剩下的因子是互质的。

把形如\\(x^{n-1}+x^{n-1}y+\cdots+y^{n-1}\\)（即，\\(\sum\_{k=0}^{n-1} x^{n-k-1} y^k\\)，其中\\(x\\)和\\(y\\)互质）的式子记作\\(f(n)\\)，则需要证明如下命题：

“若\\(p\\)与\\(q\\)互质，则\\(f(p)\\)与\\(f(q)\\)互质。”

不妨设\\(p > q\\)，则

\\[\eqalign{
f(p)
&= x^{p-1} + x^{p-2}y + \cdots + x^{p-q}y^{q-1} + x^{p-q-1}y^q + \cdots + y^{p-1} \cr
&= x^{p-q}\left(x^{q-1}+x^{q-2}y+\cdots+y^{q-1}\right) + \left(x^{p-q-1} + \cdots + y^{p-q-1}\right)y^q \cr
&= x^{p-q} f(q) + f(p-q)y^q
}\\]

要证明\\(f(p)\\)和\\(f(q)\\)互质，只需证明\\(f(q)\\)和\\(f(p-q)y^q\\)互质。

首先，\\(f(q)\\)和\\(y^q\\)一定互质，这是因为\\((x-y)f(q) = x^q-y^q\\)和\\(y^q\\)互质，因为\\(x\\)和\\(y\\)互质。

于是，只需证明\\(f(q)\\)和\\(f(p-q)\\)互质即可。令\\(p'=q\\), \\(q'=p-q\\)，显然\\(p'\\)和\\(q'\\)是互质的。接下来需要证明的命题为

“若\\(p'\\)与\\(q'\\)互质，则\\(f(p')\\)与\\(f(q')\\)互质。”

可见，和前一个命题完全相同，但是\\(p'\\)和\\(q'\\)的数值严格减小了(\\(\max\\{p',q'\\} < p\\))。反复运用上述证明过程，最终有\\(\min\\{p,q\\}=1\\)。此时命题显然成立。

综上所述，除\\(x-y\\)外，\\(x^p-y^p\\)和\\(x^q-y^q\\)没有其他公因子，所以
\\[\gcd(x^p-y^p, x^q-y^q) = x-y\\]
成立，所以原命题成立。
