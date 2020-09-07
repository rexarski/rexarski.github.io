---
title: "Circular Permutation"
date: 2019-09-10T23:10:51+10:00
slug: "circular-permutation"
description: ""
draft: false
tags: ["Maths", "Graph Theory"]
math: true
showToc: false
---

A colleague had a coding interview for Huawei last Sunday. I heard the second question was quite “mathematical”. Let me rephrase it here a little bit.

<!--more-->

> A hero summoner in a MOBA game has an ability to manipulate three elements. By controlling the order of releasing these elements, he can cast different spells accordingly. For example, casting in the order of _fire, water, lightening_ can be treated as a spell. But there are some limitations as well.

> Consider fitting the elements of that spell in a cycle. Then turning the cycle clockwise or counterclockwise does not produce any new spells. Additionally, inverting the cycle will not generate new ones either. The question is, if n is the number of elements he is capable of mastering, m is the number of elements consisting a spell, then what is the value of the number of different spells modulo 1000000007?

Typically, the mathematical term that describes the way of ordering elements, is called [Circular Permutation](http://mathworld.wolfram.com/CircularPermutation.html).

The number of ways to arrange $n$ distinct objects along a fixed (i.e., cannot be picked up out of the plane and turned over) circle is

<div>$$ P_n=(n-1)!$$ </div>

The reason why it is the factorial of {{<math "inline">}}n-1{{< /math >}} instead of $n$ is all cycle rotation.

<img class="special-img-class" style="width:100%" src="https://mathworld.wolfram.com/images/eps-gif/CircularPermutations_950.gif" />

If we consider a stricter definition, there will be only three free permutations (i.e., inequivalent when flipping the circle is allowed).

<div>$$P’_n=\frac{1}{2} (n-1)!, n\geq 3$$</div>

In our problem, the number would be

<div>$${n \choose m} \frac{1}{2} (m-1)!$$</div>

Since {{<math "inline">}}1\leq m \leq 10000, 1\leq n \leq 20000{{< /math >}}, direct calculation of factorial is suicidal for a computer. The hack here should be using modulo arithmetic, namely, we only keep the mod of {{<math "inline">}}10^9 + 7{{< /math >}} in intermediate steps.

```r
fact <- function(n) {
  res <- 1
  for (i in 1:n) {
    res <- (res * i) %% 1000000007
  }
  return(res)
}
```

Although `factorial(203)` will give us `Inf` as a result, `fact(203` won’t. It will give us an exact answer of `572421883`.
