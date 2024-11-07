---
title: "units"
linkTitle: "units"
type: "docs"
description: units 包 - 单位处理
weight: 100
---

## 单位的常量

- 定点数: `n`, `u`, `m`, `k`, `K`, `G`, `T` 和 `P`.
- 2 的幂: `Ki`, `Mi`, `Gi`, `Ti` 和 `Pi`.

## 函数列表

- `to_n(num: int) -> str`
  将 int 转换为以 `n` 作为后缀的字符串
- `to_u(num: int) -> str`
  将 int 转换为以 `u` 作为后缀的字符串
- `to_m(num: int) -> str`
  将 int 转换为以 `m` 作为后缀的字符串
- `to_K(num: int) -> str`
  将 int 转换为以 `K` 作为后缀的字符串
- `to_M(num: int) -> str`
  将 int 转换为以 `M` 作为后缀的字符串
- `to_G(num: int) -> str`
  将 int 转换为以 `G` 作为后缀的字符串
- `to_T(num: int) -> str`
  将 int 转换为以 `T` 作为后缀的字符串
- `to_P(num: int) -> str`
  将 int 转换为以 `P` 作为后缀的字符串
- `to_Ki(num: int) -> str`
  将 int 转换为以 `Ki` 作为后缀的字符串
- `to_Mi(num: int) -> str`
  将 int 转换为以 `Mi` 作为后缀的字符串
- `to_Gi(num: int) -> str`
  将 int 转换为以 `Gi` 作为后缀的字符串
- `to_Ti(num: int) -> str`
  将 int 转换为以 `Ti` 作为后缀的字符串
- `to_Pi(num: int) -> str`
  将 int 转换为以 `Pi` 作为后缀的字符串

```python
import units
# SI
n = units.to_n(1e-9)
u = units.to_u(1e-6)
m = units.to_m(1e-1)
K = units.to_K(1000)
M = units.to_M(1000000)
G = units.to_G(1000000000)
T = units.to_T(1000000000000)
P = units.to_P(1000000000000000)
# IEC
Ki = units.to_Ki(1024)
Mi = units.to_Mi(1024 ** 2)
Gi = units.to_Gi(1024 ** 3)
Ti = units.to_Ti(1024 ** 4)
Pi = units.to_Pi(1024 ** 5)
```
