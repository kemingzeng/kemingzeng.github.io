---
layout: post
title: public、private在继承时对位宽占用的坑
date: 2021-07-05
categories: blog
tags: [memory]
description: public和private竟然还有这区别。

---

#### public、private在继承时有奇怪的坑

- 当父类成员是public，子类成员无论是public还是private，他们的数据位都拼不上！
- 拼不上！

```c++
class A1 {
  public:   // public is very special
    uint32_t a : 16;
};

class A2 {
  private:
    uint32_t a : 16;
};

// 8 Bytes
class B : public A1 {  // or private
  private:  // or public
    uint32_t b : 16;
};

// 4 Bytes
class C : public A2 {  // or private
  private:             // or public
    uint32_t c : 16;
};

// 4 Bytes
class D {
  public:
    uint32_t a : 16;

  private:
    uint32_t b : 16;
};

int main() {
    cout << sizeof(B) << " " << sizeof(C) << " " << sizeof(D) << endl;
    return 0;
}
```

- 结果已经标在注释上了，非常的神奇。