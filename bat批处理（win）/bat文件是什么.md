**.bat 批处理文件** 是 Windows 系统下的一种**脚本文件**，通过包含一系列 **DOS 命令**（如 `dir`、`copy`、`echo` 等）来自动化执行任务。它的扩展名为 `.bat`（意为 "batch"，即“批处理”），由 **Windows 命令解释器（cmd.exe）** 逐行解释执行。

---

## **1. 基本特点**
- **无需编译**：直接双击或在 CMD 中运行。
- **顺序执行**：按行依次运行命令。
- **支持简单逻辑**：如 `if` 条件判断、`for` 循环、`goto` 跳转等。
- **常用于**：
  - 批量文件操作（复制、删除、重命名）。
  - 自动化软件安装/配置。
  - 定时任务（结合 Windows 任务计划程序）。

---

## **2. 如何创建和运行？**
### **（1）创建 `.bat` 文件**
1. 新建一个文本文件（如 `test.txt`）。
2. 写入命令，例如：
   ```bat
   @echo off
   echo Hello, World!
   pause
   ```
3. 将文件后缀改为 `.bat`（如 `test.bat`）。

### **（2）运行方式**
- **双击运行**（会弹出 CMD 窗口）。
- **在 CMD 中执行**：
  ```cmd
  C:\> test.bat
  ```
### 也可以：
```bush
PS D:\github-program\shell-H\bat批处理（win）> ./test.bat
Hello, World!
请按任意键继续. . . 
PS D:\github-program\shell-H\bat批处理（win）> 
```

---

## **3. 常用命令示例**
| 命令 | 说明 |
|------|------|
| `@echo off` | 关闭回显（不显示执行的命令本身） |
| `echo 文本` | 输出文本 |
| `pause` | 暂停，等待用户按键 |
| `set /p var=提示` | 接收用户输入并存储到变量 `var` |
| `if 条件 (命令)` | 条件判断 |
| `for %%i in (*.txt) do (命令)` | 循环处理文件 |
| `start 程序` | 启动某个程序 |
| `xcopy /s 源 目标` | 复制文件夹（含子目录） |

---

## **4. 实际案例**
### **（1）自动备份文件**
```bat
@echo off
xcopy C:\MyDocs\*.txt D:\Backup /s /y
echo 备份完成！
pause
```
**作用**：将 `C:\MyDocs` 下所有 `.txt` 文件备份到 `D:\Backup`。

### **（2）批量重命名文件**
```bat
@echo off
setlocal enabledelayedexpansion
set i=1
for %%f in (*.jpg) do (
    ren "%%f" "photo!i!.jpg"
    set /a i+=1
)
echo 重命名完成！
pause
```
**作用**：将所有 `.jpg` 文件按 `photo1.jpg`、`photo2.jpg`... 格式重命名。

---

## **5. 注意事项**
- **安全性**：`.bat` 文件可能包含恶意代码，**不要随意运行未知来源的批处理文件**。
- **编码问题**：默认使用 `ANSI` 编码，若含中文建议另存为 `GB2312` 或 `UTF-8（带BOM）`。
- **权限问题**：某些命令（如修改系统设置）需**管理员权限**运行（右键 → “以管理员身份运行”）。

---

## **6. 进阶：`.bat` vs `.ps1`（PowerShell）**
| **`.bat`** | **`.ps1`（PowerShell脚本）** |
|------------|-----------------------------|
| 依赖 `cmd.exe` | 依赖 PowerShell |
| 语法简单，功能有限 | 支持面向对象、.NET 集成 |
| 适合简单任务 | 适合复杂自动化、系统管理 |

如果任务较复杂（如操作注册表、网络请求），建议使用 **PowerShell**（`.ps1` 文件）。

---

### **总结**
`.bat` 是 Windows 下经典的批处理脚本，适合**快速自动化简单任务**。虽然功能不如 PowerShell 强大，但在日常文件管理、批量操作中依然非常实用。如果需要更高级的功能，可以学习 PowerShell 或 Python 脚本。