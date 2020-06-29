# CPU Migrate

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
make cpu_migrate
```

## 测试

有关的 `perf` 测试的数据，依然来自 `E5-2620 v4`

### 不绑定

``` bash
build/cpu_migrate -t 16 -s
```

输出为

``` text
avg: 836
```

#### 使用 pref 测试不绑定的缓存命中率

``` bash
perf stat -e cpu-migrations,cache-references,cache-misses,instructions,cycles,L1-dcache-load-misses,L1-dcache-loads,L1-icache-load-misses,branch-load-misses,branch-loads build/cpu_migrate -t 16 -s
```

输出结果为

``` text
 Performance counter stats for 'build/cpu_migrate -t 16 -s':

                10      cpu-migrations
         8,193,825      cache-references                                              (44.40%)
           175,792      cache-misses              #    2.145 % of all cache refs      (44.34%)
    45,480,238,906      instructions              #    1.30  insn per cycle           (55.47%)
    35,111,144,560      cycles                                                        (55.47%)
        11,997,428      L1-dcache-load-misses     #    0.05% of all L1-dcache hits    (55.57%)
    26,407,960,253      L1-dcache-loads                                               (55.60%)
         2,459,766      L1-icache-load-misses                                         (55.66%)
         2,136,304      branch-load-misses                                            (44.53%)
     3,825,848,726      branch-loads                                                  (44.43%)

       1.251076337 seconds time elapsed

      14.630618000 seconds user
       0.459616000 seconds sys
```

### 绑定

``` bash
build/cpu_migrate -t 16 -f
```

输出为

``` text
avg: 788
```

#### 使用 pref 测试绑定的缓存命中率

``` bash
perf stat -e cpu-migrations,cache-references,cache-misses,instructions,cycles,L1-dcache-load-misses,L1-dcache-loads,L1-icache-load-misses,branch-load-misses,branch-loads build/cpu_migrate -t 16 -f
```

输出结果为

``` text
 Performance counter stats for 'build/cpu_migrate -t 16 -f':

                14      cpu-migrations
         4,983,541      cache-references                                              (44.42%)
         1,611,627      cache-misses              #   32.339 % of all cache refs      (44.34%)
    45,523,818,723      instructions              #    1.52  insn per cycle           (55.43%)
    29,972,627,158      cycles                                                        (55.46%)
         5,812,831      L1-dcache-load-misses     #    0.02% of all L1-dcache hits    (55.53%)
    26,388,005,477      L1-dcache-loads                                               (55.58%)
         1,262,533      L1-icache-load-misses                                         (55.66%)
         1,363,376      branch-load-misses                                            (44.54%)
     3,828,570,015      branch-loads                                                  (44.47%)

       0.948650967 seconds time elapsed

      12.489932000 seconds user
       0.456253000 seconds sys
```
