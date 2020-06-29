# Traverse 2D Array

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
make traverse_2d_array
```

## 运行

### 步长 1

``` bash
build/traverse_1d_array -s 1
```

结果为

``` text
12,access count:8388608
```

### 步长 128

``` bash
build/traverse_1d_array -s 128
```

结果为

``` text
268,access count:8388608
```

### 步长 1024

``` bash
build/traverse_1d_array -s 1024
```

结果为

``` text
2296,access count:8388608
```
