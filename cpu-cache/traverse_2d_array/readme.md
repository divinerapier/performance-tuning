# Traverse 2D Array

## 环境

* 操作系统: windows10 wsl2 archlinux
* CPU: AMD Ryzen 7 4800HS with Radeon Graphics
* clang: version 10.0.0

## 编译

``` bash
mkdir -p build
cd build
cmake ..
make traverse_2d_array
```

## 运行

### array[i][j] 顺序

``` bash
build/traverse_2d_array -f
```

结果为

``` text
arr[i][j]
8
```

### array[j][i] 顺序

``` bash
build/traverse_2d_array -s
```

结果为

``` text
arr[j][i]
23
```

## 使用 perf

`perf state` 的部分指标还不支持 `AMD Ryzen 7`，这里找了别人 `E5-2620 v4` 的测试结果

``` bash
perf stat \
    -e cache-references,cache-misses,instructions,cycles,L1-dcache-load-misses,L1-dcache-loads \
    build/traverse_2d_array -f
```

输出结果

``` text
 Performance counter stats for './traverse_2d_array -f':

           147,927      cache-references                                              (80.14%)
            13,215      cache-misses              #    8.933 % of all cache refs      (65.49%)
        54,454,827      instructions              #    1.43  insn per cycle           (85.11%)
        38,197,267      cycles                                                        (85.09%)
           161,503      L1-dcache-load-misses     #    0.90% of all L1-dcache hits    (85.09%)
        18,035,307      L1-dcache-loads                                               (84.19%)

       0.020651344 seconds time elapsed

       0.018625000 seconds user
       0.002069000 seconds sys
```

``` bash
perf stat \
    -e cache-references,cache-misses,instructions,cycles,L1-dcache-load-misses,L1-dcache-loads \
    build/traverse_2d_array -f
```

输出结果

``` text
 Performance counter stats for './traverse_2d_array -s':

         4,341,186      cache-references                                              (83.01%)
            13,974      cache-misses              #    0.322 % of all cache refs      (66.03%)
        55,245,646      instructions              #    0.25  insn per cycle           (83.01%)
       218,787,967      cycles                                                        (83.00%)
         4,308,394      L1-dcache-load-misses     #   23.79% of all L1-dcache hits    (83.86%)
        18,112,753      L1-dcache-loads                                               (84.10%)

       0.082950118 seconds time elapsed

       0.079066000 seconds user
       0.003953000 seconds sys
```
