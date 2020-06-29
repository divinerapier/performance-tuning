# Branch Predict

## 环境

* 操作系统: windows10 wsl2 archlinux
* CPU: AMD Ryzen 7 4800HS with Radeon Graphics
* clang: version 10.0.0

## 编译

``` bash
mkdir -p build
cd build
export CC=$(which clang)
export CXX=$(which clang++)
cmake ..
make branch_predict
```

## 测试

有关的 `perf` 测试的数据，依然来自 `E5-2620 v4`

### 生成数据

``` bash
build/branch_predict -g
```

### 随机遍历数组

``` bash
build/branch_predict -s
```

输出为

``` text
598
```

#### 使用 pref 验证随机遍历的命中率

``` bash
perf stat -e cache-references,cache-misses,instructions,cycles,L1-dcache-load-misses,L1-dcache-loads ./branch_predict -s
```

输出为

``` text
 Performance counter stats for './branch_predict -s':

     1,504,222,463      instructions              #    0.49  insn per cycle
     3,084,103,065      cycles
            84,992      L1-icache-load-misses
        67,153,573      branch-load-misses
       273,893,334      branch-loads

       1.040296121 seconds time elapsed

       0.987243000 seconds user
       0.053060000 seconds sys
```

### 顺序遍历数组


``` bash
build/branch_predict -f
```

输出为

``` text
207
```

#### 使用 pref 验证顺序遍历的命中率

``` bash
perf stat -e cache-references,cache-misses,instructions,cycles,L1-dcache-load-misses,L1-dcache-loads ./branch_predict -f
```

输出为

``` text
 Performance counter stats for './branch_predict -f':

     1,501,478,360      instructions              #    1.27  insn per cycle
     1,180,637,963      cycles
            49,351      L1-icache-load-misses
            27,282      branch-load-misses
       273,404,227      branch-loads

       0.404512198 seconds time elapsed

       0.352686000 seconds user  
       0.051845000 seconds sys
```
