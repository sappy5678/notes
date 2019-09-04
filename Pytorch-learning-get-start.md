---
title: Pytorch 學習筆記
categories: [深度學習,Pytorch,教學]
tags: [深度學習,Pytorch,教學]
date: 2018-03-06 22:44:40
---

# 資源
* [官方教學 - 六十分鐘學會 Pytorch](http://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)  
* [六十分鐘學會 Pytorch - 中文翻譯](https://zhuanlan.zhihu.com/p/25572330)  
* [六十分鐘學會 Pytorch - 我的畫注](https://goo.gl/YBKk2s)

* [深度学习框架PyTorch：入门与实践 - 程式碼](https://github.com/chenyuntc/pytorch-book)

這裡補充一些 Variable 的小東西
```python
a = Variable(torch.Tensor([3]), requires_grad = True)
b = Variable(torch.Tensor([4]), requires_grad = True)


print(a,b)

d = a.pow(2) + b # a^2 + b
```

可以透過 算出 梯度
```pytohn
d.backward()
print(a.grad)
print(b.grad)
```

輸出是
```output
Variable containing:
 6
[torch.FloatTensor of size 1]

Variable containing:
 1
[torch.FloatTensor of size 1]
```

透過 grad_fn 可以看出計算圖(Computation Graph)長成甚麼樣子
```python
print(d.grad_fn)
print(d.grad_fn.next_functions)
```


輸出
```output
<AddBackward1 object at 0x7f73196e5eb8>
((<PowBackward0 object at 0x7f73196e5ba8>, 0), (<AccumulateGrad object at 0x7f73196e5b38>, 0))
```


