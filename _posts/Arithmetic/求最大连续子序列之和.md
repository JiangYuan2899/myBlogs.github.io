---
layout: post
title: 求最大连续子序列之和
category: other
tags: [other]
---

## 求最大连续子序列之和
//动态规划算法
````  
int MaxSum(int n)
{
int sum=0,b=0;
for(int i=0;i<n;i++)
{
if(b>0) {
        b+=a[i];}
else {b=a[i];}
if(b>sum) sum=b;
}
return sum;
}
````` 