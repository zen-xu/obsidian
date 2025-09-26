---
tags:
  - cs
---
## 常用 bit 位操作技巧

### 1. 检查第 k 位 bit 是否被设置为 1

```python
def is_kth_bit_set(n: int, k: int) -> bool:
	return (n & (1<<k)) != 0
```

### 2. 设置第 k 位 bit 为 1

```python
def set_kth_bit(n: int, k: int) -> int:
	return n | 1 << k
```

### 3. 清除第 k 位 bit 为 0

```python
def clear_kth_bit(n: int, k: int) -> int:
	return n & (~(1 << k))
```

### 4. 翻转第 k 位 bit

```python
def toggle_kth_bit(n: int, k: int) -> int:
	return n ^ (1 << k)
```

### 5. 检查是否是奇偶

```python
def is_odd(n: int) -> bool:
	return (n & 1) == 1

def is_even(n: int) -> bool:
	return (n & 1) == 0
```

### 6. 乘以/除以 2 的幂次方

```python
num = 10
num * 4 == num << 2
num // 4 == num >> 2
```

### 7. 不依靠临时变量交换 2 个数字

```python
a = 1
b = 2
a = a ^ b
b = a ^ b
a = a ^ b
print(a, b)  # 2, 1
```

### 8.  bit 为 1 计数

```python
def bitcount(n: int) -> int:
	count = 0
	while n:
		n &= n - 1
		count += 1
	return count
```

### 9. 判断是否为 2 的幂

```python
def is_power_of_two(n: int) -> bool:
	return n > 0 and (n & (n-1)) == 0
```

### 10. 清除从最低有效位到第 k 位（含）的所有位

```python
def clear_bits_LSB_to_k(n: int, k: int) -> int:
	mask = ~((1 << (k+1)) - 1)
	return n & mask
```