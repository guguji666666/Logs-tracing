# 📥 Capture LDAP Logs for Troubleshooting

This guide walks you through collecting LDAP-related logs using **Process Monitor** and **Network Trace (NMCap)** for deep-dive troubleshooting.

---

## 🔍 1. Capture Logs with Process Monitor

### Step 1: [Download Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)

### Step 2: Launch Process Monitor as Administrator

### Step 3: Configure Filters to Capture LDAP Events

* Open the **Filter** settings in Process Monitor.
* Add a filter keyword for `:ldap` to capture relevant traffic.
* Add a filter keyword for `msft-gc` to capture relevant traffic.

![Procmon LDAP Filter](https://github.com/user-attachments/assets/aeedb5a3-2ef6-4c2b-8205-71ef8aed8968)

```
:ldap
```
```
msft-gc
```

Example filter view:

![Procmon Filter](https://github.com/user-attachments/assets/8857d58e-84f5-4eeb-9dd9-af1dc815cdb3)

Result preview:

![Procmon Results](https://github.com/user-attachments/assets/3bf22dc8-bbc3-4d83-9910-8379b79ec23a)

![image](https://github.com/user-attachments/assets/95fdaf6a-71a8-4af6-b7c7-82d6e24b2fc2)


---

### Step 4: Limit Log File Size

* Go to **File > Backing Files...**
* Enable circular logging and set the **maximum file size** (e.g., 1 GB).

> Once the file size limit is reached, older data is overwritten to ensure continuous logging.

![Log Size Setting](https://github.com/user-attachments/assets/a7a585b3-f802-4f9e-9ae7-d46b26b7f0a1)

![Max Size](https://github.com/user-attachments/assets/24655d8c-15b9-457d-a892-49b1151af058)

---

### Step 5: Simulate LDAP Activity with PowerShell

Run the following PowerShell script to generate LDAP events (make sure to run as Administrator once to register the event source):

```powershell
# Add a custom event log source
if (-not [System.Diagnostics.EventLog]::SourceExists("MyLDAPScript")) {
    New-EventLog -LogName Application -Source "MyLDAPScript"
}

# Function to write to the event log
function Log-Event {
    param (
        [string]$message,
        [string]$eventType = "Information"
    )
    Write-EventLog -LogName Application -Source "MyLDAPScript" -EventID 1000 -EntryType $eventType -Message $message
}

# Perform LDAP query
$rootDSE = [ADSI]"LDAP://RootDSE"
$domainDN = $rootDSE.defaultNamingContext

$searcher = New-Object System.DirectoryServices.DirectorySearcher
$searcher.SearchRoot = [ADSI]"LDAP://$domainDN"
$searcher.Filter = "(objectClass=trustedDomain)"
$searcher.PropertiesToLoad.Add("cn") | Out-Null
$searcher.PropertiesToLoad.Add("trustPartner") | Out-Null

try {
    $results = $searcher.FindAll()
    Log-Event -message "LDAP search executed successfully."

    foreach ($result in $results) {
        $entry = $result.GetDirectoryEntry()
        [PSCustomObject]@{
            CN           = $entry.cn
            TrustPartner = $entry.trustPartner
        }
    }
} catch {
    Log-Event -message "An error occurred during LDAP search: $_" -eventType "Error"
    Write-Error "An error occurred: $_"
}
```

---

## 🌐 2. Capture Network Trace (LDAP Ports)

### Step 1: Run the Following Command as Administrator

```cmd
mkdir c:\mslogs

NMCap /network * /capture "(tcp.port == 389 or tcp.port == 636 or tcp.port==3268 or tcp.port == 3269)" /file c:\mslogs\test.cap:300M
```

### 📌 Explanation:

* `NMCap` is the network capture utility.
* It monitors all interfaces: `/network *`
* It captures traffic on common LDAP ports: 389 (LDAP), 636 (LDAPS), 3268/3269 (Global Catalog)
* The `:300M` parameter restricts each capture file to 300 MB. You can adjust this value as needed (e.g., 100M for light logs or 1G for heavy usage).

Example in action:
![NMCap Example](https://github.com/user-attachments/assets/a4f7bd5d-989c-47b0-a4fd-26385ec57517)

> ⚠️ **Important:**
>
> * **Do NOT close the CMD window** while the capture is running.
> * **Do NOT sign out or reboot the machine**, as this will interrupt the capture.
> * When the issue has been reproduced, press **Ctrl + C** in the CMD window to stop the trace.

---

## 📤 After Reproducing the Issue

Once the issue is reproduced:

1. Stop Process Monitor and save the `.PML` file.
2. Stop NMCap using `Ctrl + C`, and keep the `test.cap` file.
3. Collect the following:

   * Process Monitor logs (`.pml`)
   * Network trace (`c:\mslogs\test.cap`)
   * Any scan or diagnostic reports (if applicable)
4. Share the logs for further analysis.

---


以下是上述内容的中文版，内容结构优化、美化，适合用于技术支持文档或内部知识库：

---

# 📥 捕获 LDAP 日志指南

本指南将引导您通过 **Process Monitor** 和 **网络抓包（NMCap）** 工具，采集与 LDAP 相关的活动日志，帮助进行故障排查和深入分析。

---

## 🔍 一、使用 Process Monitor 捕获日志

### 步骤 1：[下载 Process Monitor](https://learn.microsoft.com/zh-cn/sysinternals/downloads/procmon)

### 步骤 2：以管理员身份运行 Process Monitor

### 步骤 3：设置过滤器，捕获与 LDAP 相关的事件

* 打开菜单 **Filter > Filter...**
* 添加过滤关键字 `ldap` 和 `msft-gc`，用于匹配相关操作

![Procmon LDAP Filter](https://github.com/user-attachments/assets/aeedb5a3-2ef6-4c2b-8205-71ef8aed8968)

```
:ldap
```

```
msft-gc
```

过滤设置示例：

![Procmon Filter](https://github.com/user-attachments/assets/8857d58e-84f5-4eeb-9dd9-af1dc815cdb3)

匹配结果示例：

![Procmon Results](https://github.com/user-attachments/assets/3bf22dc8-bbc3-4d83-9910-8379b79ec23a)

![image](https://github.com/user-attachments/assets/7e5f0558-1543-4783-bf0e-855f7f1d3290)

---

### 步骤 4：限制日志文件大小

* 进入 **File > Backing Files...**
* 勾选启用环形日志（circular logging）
* 设置最大文件大小（如示例中设为 1 GB）

> 达到最大大小后，旧日志将被新数据自动覆盖，从而避免日志无限增长。

示意图：

![Log Size Setting](https://github.com/user-attachments/assets/a7a585b3-f802-4f9e-9ae7-d46b26b7f0a1)

![Max Size](https://github.com/user-attachments/assets/24655d8c-15b9-457d-a892-49b1151af058)

---

### 步骤 5：通过 PowerShell 模拟 LDAP 查询行为（可用于测试）

可运行以下 PowerShell 脚本生成 LDAP 查询行为，并写入应用程序事件日志（首次运行请使用管理员权限）：

```powershell
# 注册事件源
if (-not [System.Diagnostics.EventLog]::SourceExists("MyLDAPScript")) {
    New-EventLog -LogName Application -Source "MyLDAPScript"
}

# 定义写日志函数
function Log-Event {
    param (
        [string]$message,
        [string]$eventType = "Information"
    )
    Write-EventLog -LogName Application -Source "MyLDAPScript" -EventID 1000 -EntryType $eventType -Message $message
}

# 查询 RootDSE
$rootDSE = [ADSI]"LDAP://RootDSE"
$domainDN = $rootDSE.defaultNamingContext

# 创建 DirectorySearcher 对象
$searcher = New-Object System.DirectoryServices.DirectorySearcher
$searcher.SearchRoot = [ADSI]"LDAP://$domainDN"
$searcher.Filter = "(objectClass=trustedDomain)"
$searcher.PropertiesToLoad.Add("cn") | Out-Null
$searcher.PropertiesToLoad.Add("trustPartner") | Out-Null

try {
    $results = $searcher.FindAll()
    Log-Event -message "LDAP 查询执行成功"

    foreach ($result in $results) {
        $entry = $result.GetDirectoryEntry()
        [PSCustomObject]@{
            CN           = $entry.cn
            TrustPartner = $entry.trustPartner
        }
    }
} catch {
    Log-Event -message "LDAP 查询出错: $_" -eventType "Error"
    Write-Error "发生错误: $_"
}
```

---

## 🌐 二、使用 NMCap 抓取网络层 LDAP 报文

### 步骤 1：以管理员身份运行以下命令

```cmd
mkdir c:\mslogs

NMCap /network * /capture "(tcp.port == 389 or tcp.port == 636 or tcp.port==3268 or tcp.port == 3269)" /file c:\mslogs\test.cap:300M
```

### 📌 命令说明：

* `NMCap` 是网络抓包工具
* `/network *` 表示抓取所有网络接口
* 捕获常见的 LDAP 端口：389（LDAP）、636（LDAPS）、3268/3269（全局编录服务）
* `:300M` 表示将单个日志文件大小限制为 300 MB（可调整为 100M、1G 等）

示例运行效果：
![NMCap Example](https://github.com/user-attachments/assets/a4f7bd5d-989c-47b0-a4fd-26385ec57517)

> ⚠️ **注意事项：**
>
> * **不要关闭 CMD 窗口**，否则抓包会中断。
> * **不要注销或重启系统**，否则需要重新部署命令。
> * 问题复现后，请在 CMD 中按下 **Ctrl + C** 终止抓包。

---

## ✅ 问题复现后请及时收集日志

当问题复现后，请完成以下操作：

1. 停止 Process Monitor，并保存 `.PML` 日志文件
2. 使用 `Ctrl + C` 停止 NMCap 抓包，保留 `c:\mslogs\test.cap` 文件
3. 将以下内容统一打包并发送：

   * Process Monitor 日志 (`.pml`)
   * NMCap 抓包日志 (`test.cap`)
   * 扫描报告或其他相关诊断信息（如适用）

---
