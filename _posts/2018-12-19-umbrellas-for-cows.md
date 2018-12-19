---
layout: post
title: Umbrellas for Cows (USACO December 2012 Silver Div.)
categories:
  - Competitive Programming
---

Problem: <http://www.usaco.org/index.php?page=viewproblem2&cpid=99>

This is a dynamic programming problem. We define \\( d_{i} \\) as the minimum cost it takes to protect all cows numbered from $$1$$ to $$i$$. We compute $$d_i$$ from $$i=1$$ to $$n$$.

Note that the larger umbrella can be cheaper than the smaller umbrella. We denote the cost of the cheapest umbrella that can protects the area with width $$w$$ as $$c^\prime_w$$. It is obtained by the following formula.

$$c_w^{'} = \min_{w \le i \le m} (c_{w})$$


$$d_1$$ is obtained by taking the minimum value of $$C_w$$ where $$1 \le i \le m$$. Remaining $$d_i$$s is calculated by the following formula.

d_{i}= \min (d_{i-1}, C^'_i)

where 2 \le i \le m and C^'_i is the minimum value of C_k where 1 \le k \le m

wefwe
