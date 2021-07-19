---
layout: post
title: 查看内存占用
date: 2021-07-08
categories: blog
tags: [develop]
description: 查看内存占用。
---

#### 0. /bin/time

```shell
/bin/time --verbose xxx
```

- 这个命令最好用，但是看不到虚拟内存，需要top去看

#### 1. 嵌入代码

- 从/proc/self去查找

```c++
#include <iomanip> 
#include <fstream>

using namespace std;

void formatValue(const char msg[], double value, int index) {
    string memory_unit[5] = {" [B]", " [KB]", " [MB]", " [GB]", " [TB]"};
    while (value > 900) {
        value /= 1024;
        index++;
    }
    value < 2 ? cout << fixed << setprecision(2) : cout << fixed << setprecision(1);
    cout << msg << value << memory_unit[index] << endl;
}
static int printMemoryInfo() {
    ifstream fin("/proc/self/status");
    if (!fin) {
        cout << "memory info unknown!\n";
    } else {
        string part;
        double value;
        cout << fixed;
        while (fin >> part) {
            if (part == "VmHWM:") {
                fin >> value;
                formatValue("peak real memory usage: ", value, 1);
            } else if (part == "VmPeak:") {
                fin >> value;
                formatValue("peak virtual memory usage: ", value, 1);
            } else if (part == "VmRSS:") {
                fin >> value;
                formatValue("current real memory usage: ", value, 1);
            } else if (part == "VmSize:") {
                fin >> value;
                formatValue("current virtual memory usage: ", value, 1);
            }
        }
        fin.close();
        cout.unsetf(ios::fixed);
        cout.precision(6);
    }
    return 0;
}
```

