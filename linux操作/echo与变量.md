# echo与变量

2025.6.4

---
# Linux `echo` 命令与变量使用笔记

## 一、`echo` 命令基础

`echo` 是 Linux 中最常用的命令之一，用于在终端输出文本或变量内容。

### 基本语法
```bash
echo [选项] [字符串/变量]
```

### 常用选项
| 选项 | 说明 |
|------|------|
| `-n` | 不自动添加换行符 |
| `-e` | 启用转义字符解释 |
| `-E` | 禁用转义字符解释（默认） |

### 示例
```bash
echo "Hello World"      # 输出 Hello World 并换行
echo -n "No newline"    # 输出不换行
echo -e "Line1\nLine2"  # 输出两行，使用 \n 换行
```

## 二、Linux 变量基础

### 1. 变量定义与使用
```bash
变量名=值       # 定义变量（注意等号两边不能有空格）
echo $变量名    # 使用变量
```

### 示例
```bash
name="Alice"
echo $name      # 输出 Alice
echo "$name"    # 输出 Alice（推荐带引号）
echo '$name'    # 输出 $name（单引号不解析变量）
```

### 2. 变量类型
- **局部变量**：仅在当前 shell 有效
- **环境变量**：可被子进程继承（使用 `export`）
- **特殊变量**：
  - `$0`：脚本名称
  - `$1-$9`：脚本参数
  - `$#`：参数个数
  - `$?`：上条命令退出状态
  - `$$`：当前进程 PID

## 三、`echo` 与变量高级用法

### 1. 命令替换
```bash
echo "Today is $(date)"      # $(命令) 形式
echo "Today is `date`"       # 反引号形式（旧式）
```

### 2. 变量操作
```bash
path="/usr/local/bin"
echo ${path}           # 基本用法
echo ${path#/usr}      # 删除最短匹配前缀（输出 /local/bin）
echo ${path##*/}       # 删除最长匹配前缀（输出 bin）
echo ${path%/*}        # 删除最短匹配后缀（输出 /usr/local）
echo ${path%%/*}       # 删除最长匹配后缀（输出 空）
```

### 3. 数组变量
```bash
colors=("red" "green" "blue")
echo ${colors[1]}      # 输出 green（索引从0开始）
echo ${colors[@]}      # 输出所有元素
echo ${#colors[@]}     # 输出数组长度
```

## 四、实用技巧

### 1. 彩色输出
```bash
echo -e "\033[31mRed Text\033[0m"  # 红色文字
echo -e "\033[42mGreen Background\033[0m"  # 绿色背景
```

### 2. 调试脚本
```bash
echo "DEBUG: value of var is $var"  # 调试时输出变量值
```

### 3. 生成文件内容
```bash
echo "line1" > file.txt        # 覆盖写入
echo "line2" >> file.txt       # 追加写入
```

## 五、注意事项

1. 变量赋值时等号两边**不能有空格**
2. 变量名区分大小写
3. 推荐给变量引用加双引号（防止空格分隔问题）
4. 单引号中的变量不会被解析
5. 使用 `{}` 明确变量边界（如 `${var}file`）

## 六、综合示例

```bash
#!/bin/bash

# 定义变量
user=$(whoami)
time=$(date +%H:%M)
dirs=("/bin" "/usr/bin" "/sbin")

# 输出信息
echo -e "\033[34m=== 系统信息 ===\033[0m"
echo "当前用户: $user"
echo "当前时间: $time"
echo -e "PATH目录:\n${dirs[@]}"

# 变量操作示例
path="/usr/local/share/doc"
echo -e "\n路径操作示例:"
echo "原始路径: $path"
echo "父目录: ${path%/*}"
echo "最后一段: ${path##*/}"
```

通过掌握 `echo` 和变量的使用，可以大大提高 shell 脚本编写的效率和灵活性。


---

# Linux 变量删除方法总结

## 一、删除普通变量

### 1. 使用 `unset` 命令（推荐方式）
```bash
unset 变量名
```

**示例**：
```bash
name="Alice"
echo $name    # 输出 Alice
unset name
echo $name    # 输出空（变量已删除）
```

### 2. 赋空值（不完全删除）
```bash
变量名=
```

**示例**：
```bash
age=25
age=
echo $age    # 输出空（变量仍存在但值为空）
```

## 二、删除环境变量

### 1. 使用 `unset`
```bash
unset 变量名
```

**示例**：
```bash
export PATH=$PATH:/new/path
unset PATH    # 删除整个PATH变量（危险操作！）
```

### 2. 从导出列表中移除（保持变量但取消导出）
```bash
export -n 变量名
```

**示例**：
```bash
export MY_VAR="test"
export -n MY_VAR  # MY_VAR变为普通变量
```

## 三、删除数组变量

### 1. 删除整个数组
```bash
unset 数组名
```

**示例**：
```bash
colors=("red" "green" "blue")
unset colors
echo ${colors[@]}  # 输出空
```

### 2. 删除数组元素
```bash
unset 数组名[索引]
```

**示例**：
```bash
fruits=("apple" "banana" "orange")
unset fruits[1]    # 删除第二个元素
echo ${fruits[@]}  # 输出 apple orange
```

## 四、删除只读变量

### 1. 常规方法无法删除
```bash
readonly PI=3.14
unset PI    # 报错：bash: unset: PI: cannot unset: readonly variable
```

### 2. 解决方法
需要退出当前shell或子shell才能解除

**示例**：
```bash
(
  readonly TEMP=100
  echo $TEMP
)  # 在子shell中创建只读变量
# 子shell结束后自动解除
```

## 五、注意事项

1. **系统变量慎删**：如 `PATH`, `HOME` 等删除可能导致系统异常
2. **检查变量是否存在**：
   ```bash
   if [ -z "${var+x}" ]; then
     echo "变量不存在"
   else
     echo "变量存在"
   fi
   ```
3. **函数删除**：`unset -f 函数名` 可以删除函数
4. **影响范围**：`unset` 只影响当前shell，不影响父进程

## 六、实用技巧

### 1. 批量删除匹配变量
```bash
for var in ${!MY_*}; do unset "$var"; done  # 删除所有MY_开头的变量
```

### 2. 安全删除检查
```bash
var_to_delete="TEMP"
if [ -n "${!var_to_delete}" ]; then
  unset "$var_to_delete"
  echo "已删除变量 $var_to_delete"
else
  echo "变量不存在"
fi
```

### 3. 删除后确认
```bash
unset var && echo "删除成功" || echo "删除失败"
```

## 七、综合示例

```bash
#!/bin/bash

# 设置测试变量
USER_DATA="important"
TEMP_FILE="/tmp/test"
LOG_LEVEL=3
readonly MAX_RETRY=5

# 显示初始变量
echo "删除前:"
set | grep -E 'USER_DATA|TEMP_FILE|LOG_LEVEL|MAX_RETRY'

# 删除操作
unset USER_DATA
unset TEMP_FILE
LOG_LEVEL=
# unset MAX_RETRY  # 这行会报错

echo -e "\n删除后:"
set | grep -E 'USER_DATA|TEMP_FILE|LOG_LEVEL|MAX_RETRY'

# 删除数组示例
services=("web" "db" "cache")
unset services[1]
echo -e "\n剩余服务: ${services[@]}"
```

掌握正确的变量删除方法可以避免脚本中的内存泄漏和变量污染问题。
