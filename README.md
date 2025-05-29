# 🧭 Capture Different Network Tracing Logs

## 🌐 1. Capture HAR Logs in Edge/Chrome

### 🧩 Step-by-Step:

1. **Launch Edge or Chrome** browser

2. Go to `... → More tools → Developer Tools` or press `F12`

3. Select the `Network` tab <br>
   ![Network Tab](https://user-images.githubusercontent.com/96930989/212235788-5cf820a0-a909-44e9-8a8f-e39f1c7a9df6.png)

4. Ensure **"Preserve Log"** is checked <br>
   ![Preserve Log](https://user-images.githubusercontent.com/96930989/212235841-de012d95-7602-45c5-8561-e4d817fd1507.png)

5. Click the **record icon** (or press `Ctrl + E`) to start/stop capture <br>
   ![Record Icon](https://user-images.githubusercontent.com/96930989/212235902-68a27a79-9ed0-4e25-855f-545190ced37e.png)

6. Click **"Export HAR…"** to save the session log <br>
   ![Export HAR](https://user-images.githubusercontent.com/96930989/212235965-a384ebf9-63a9-441a-9f64-9f83ca58c5ad.png)

---

## 🧪 2. Capture Tracing Logs in Fiddler (Windows)

### 📥 Download:

* [Fiddler Classic](http://www.telerik.com/download/fiddler)

![Fiddler](https://user-images.githubusercontent.com/96930989/227784190-16f3cb11-8822-4b3d-bfcd-09bd6ba29b1e.png)

### ⚙️ Setup:

1. Open **Fiddler 4**

2. Navigate to `Tools > Options` <br>
   ![Options](https://user-images.githubusercontent.com/96930989/227168418-ea792ab4-10cd-49de-93cf-cafd47fefa3b.png)

3. Go to the **HTTPS tab**

4. Enable `Capture HTTPS CONNECTs` and `Decrypt HTTPS traffic` <br>
   ![HTTPS Settings](https://user-images.githubusercontent.com/96930989/227168518-b7ccd9e7-a969-46b9-a696-d94a32b1dc4e.png)

5. Accept the certificate prompts <br>
   ![Confirm 1](https://user-images.githubusercontent.com/96930989/227168593-04f7fee2-562b-471e-9216-17c62915828f.png)
   
   ![Confirm 2](https://user-images.githubusercontent.com/96930989/227168640-8fc2d7fb-9825-4706-901b-2f4b21dab3b0.png)

7. Add protocols `tls1.1` and `tls1.2` under `Protocols` <br>
   ![Protocols 1](https://user-images.githubusercontent.com/96930989/227168699-3786939b-8168-47e0-9389-c0bd3cd39962.png)
   
   ![Protocols 2](https://user-images.githubusercontent.com/96930989/227168717-e3d0886c-4269-4358-b943-7b067bf6c15b.png)

8. Reproduce the issue 2–3 times

9. Go to `File > Save > Save All Sessions` and export the log

---

## 📡 3. Capture Logs in Microsoft Network Monitor

1. Install [Microsoft Network Monitor](https://www.microsoft.com/en-us/download/details.aspx?id=4865)

2. Launch as **Administrator** <br>
   ![Launch](https://github.com/user-attachments/assets/0f52568b-0eca-4cea-b49f-e3eec10a452d)

3. Click `New Capture` <br>
   ![New Capture](https://github.com/user-attachments/assets/bf8111e4-9b0c-4186-b28d-8b5d49e3643f)

4. Click `Start`, reproduce the issue <br>
   ![Start Capture](https://github.com/user-attachments/assets/d8a3b973-cf42-4947-9d92-ab8f5806eb4b)

5. Click `Stop` <br>
   ![Stop Capture](https://github.com/user-attachments/assets/5340bea6-c621-4f48-a34c-5de8e6936879)

6. Save the capture file <br>
   ![Save](https://github.com/user-attachments/assets/8634e327-46a3-4aa6-877a-159323141cf9)

---

# 💻 Capture Local Logs from Windows Machine

---

## 🪟 1. Capture Windows Update Logs

```powershell
Get-WindowsUpdateLog
```

<img width="702" alt="UpdateLog1" src="https://user-images.githubusercontent.com/96930989/233538875-22b3e56b-155f-4b7e-a949-ff8ec066bf37.png">

<img width="964" alt="UpdateLog2" src="https://user-images.githubusercontent.com/96930989/233538942-32e38838-6737-4364-b0f5-6251d092a1ca.png">

<img width="1022" alt="UpdateLog3" src="https://user-images.githubusercontent.com/96930989/233539001-05fe603d-4097-43b6-8c63-994b36adeb5b.png">


---

## 🖥️ 2. Capture Azure Arc Machine Logs

1. Create folder `C:\temp`
2. Launch `cmd` as **administrator** and run:

```cmd
cd C:\temp
azcmagent logs --full
```

### 🗂️ This gathers logs from:

```
%ProgramData%\AzureConnectedMachineAgent\Log\
%ProgramData%\GuestConfig\arc_policy_logs\
%ProgramData%\GuestConfig\ext_mgr_logs\
%ProgramData%\GuestConfig\extension_logs\
%ProgramData%\GuestConfig\extension_reports\
C:\Packages\Plugins\
```

📁 The output file will be saved to `C:\temp` as `azcmagent-logs-<datetime>-<vmname>.zip`

![LogZip1](https://user-images.githubusercontent.com/96930989/233914433-e6028bf4-23ce-4022-a32a-2724b0185fcb.png)

![LogZip2](https://user-images.githubusercontent.com/96930989/233914543-fd878455-92d5-48e5-b269-f4f221bcfa20.png)

### 📝 (Optional) Collect additional Event Logs:

* Application
* System
* Operations Manager (if related to Log Analytics or Dependency Agent)

```powershell
$logs = @("Application", "System", "Operations Manager")
foreach ($log in $logs) {
    $output = "C:\temp\$log.evtx"
    wevtutil epl $log $output
    Write-Output "Exported $log log to $output"
}
```
✔️ This will export:
* Application log → C:\temp\Application.evtx
* System log → C:\temp\System.evtx
* Operations Manager log (if exists) → C:\temp\Operations Manager.evtx
---

## 🛡️ 3. Capture Defender Logs

---

# 🧭 捕获不同类型的网络追踪日志（中文版）

## 🌐 1. 在 Edge/Chrome 浏览器中捕获 HAR 日志

### 🧩 操作步骤：

1. 打开 Edge 或 Chrome 浏览器
2. 依次点击 `... → 更多工具 → 开发者工具`，或直接按下 `F12`
3. 选择 **Network** 标签页
   ![Network Tab](https://user-images.githubusercontent.com/96930989/212235788-5cf820a0-a909-44e9-8a8f-e39f1c7a9df6.png)
4. 勾选 **Preserve Log（保留日志）**
   ![Preserve Log](https://user-images.githubusercontent.com/96930989/212235841-de012d95-7602-45c5-8561-e4d817fd1507.png)
5. 点击 **录制图标**（或按 `Ctrl + E`）开始/停止日志记录
   ![Record Icon](https://user-images.githubusercontent.com/96930989/212235902-68a27a79-9ed0-4e25-855f-545190ced37e.png)
6. 点击 **"Export HAR..."** 以导出日志文件
   ![Export HAR](https://user-images.githubusercontent.com/96930989/212235965-a384ebf9-63a9-441a-9f64-9f83ca58c5ad.png)

---

## 🧪 2. 使用 Fiddler 捕获日志（Windows）

### 📥 下载地址：

* [Fiddler Classic](http://www.telerik.com/download/fiddler)
  ![Fiddler](https://user-images.githubusercontent.com/96930989/227784190-16f3cb11-8822-4b3d-bfcd-09bd6ba29b1e.png)

### ⚙️ 配置步骤：

1. 打开 **Fiddler 4**
2. 点击菜单栏 `Tools > Options`
   ![Options](https://user-images.githubusercontent.com/96930989/227168418-ea792ab4-10cd-49de-93cf-cafd47fefa3b.png)
3. 选择 **HTTPS** 标签页
4. 勾选 `Capture HTTPS CONNECTs` 和 `Decrypt HTTPS traffic`
   ![HTTPS Settings](https://user-images.githubusercontent.com/96930989/227168518-b7ccd9e7-a969-46b9-a696-d94a32b1dc4e.png)
5. 接受证书提示
   ![Confirm 1](https://user-images.githubusercontent.com/96930989/227168593-04f7fee2-562b-471e-9216-17c62915828f.png)
   ![Confirm 2](https://user-images.githubusercontent.com/96930989/227168640-8fc2d7fb-9825-4706-901b-2f4b21dab3b0.png)
6. 在 `Protocols` 中添加 `tls1.1` 和 `tls1.2`
   ![Protocols 1](https://user-images.githubusercontent.com/96930989/227168699-3786939b-8168-47e0-9389-c0bd3cd39962.png)
   ![Protocols 2](https://user-images.githubusercontent.com/96930989/227168717-e3d0886c-4269-4358-b943-7b067bf6c15b.png)
7. 重现问题 2–3 次
8. 导出日志：点击 `File > Save > Save All Sessions`

---

## 📡 3. 使用 Microsoft Network Monitor 捕获日志

1. 下载并安装 [Microsoft Network Monitor](https://www.microsoft.com/en-us/download/details.aspx?id=4865)
2. 以管理员身份运行
   ![Launch](https://github.com/user-attachments/assets/0f52568b-0eca-4cea-b49f-e3eec10a452d)
3. 点击 `New Capture`
   ![New Capture](https://github.com/user-attachments/assets/bf8111e4-9b0c-4186-b28d-8b5d49e3643f)
4. 点击 `Start` 开始捕获，然后重现问题
   ![Start Capture](https://github.com/user-attachments/assets/d8a3b973-cf42-4947-9d92-ab8f5806eb4b)
5. 点击 `Stop` 停止捕获
   ![Stop Capture](https://github.com/user-attachments/assets/5340bea6-c621-4f48-a34c-5de8e6936879)
6. 点击保存按钮保存日志
   ![Save](https://github.com/user-attachments/assets/8634e327-46a3-4aa6-877a-159323141cf9)

---

# 💻 从 Windows 本地机器收集日志

## 🪟 1. 获取 Windows 更新日志

```powershell
Get-WindowsUpdateLog
```

<img width="702" alt="UpdateLog1" src="https://user-images.githubusercontent.com/96930989/233538875-22b3e56b-155f-4b7e-a949-ff8ec066bf37.png"> 

<img width="964" alt="UpdateLog2" src="https://user-images.githubusercontent.com/96930989/233538942-32e38838-6737-4364-b0f5-6251d092a1ca.png">  

<img width="1022" alt="UpdateLog3" src="https://user-images.githubusercontent.com/96930989/233539001-05fe603d-4097-43b6-8c63-994b36adeb5b.png">

---

## 🖥️ 2. 获取 Azure Arc 相关日志

1. 创建文件夹 `C:\temp`
2. 管理员权限运行命令行：

```cmd
cd C:\temp
azcmagent logs --full
```

📁 日志将保存在 `C:\temp\azcmagent-logs-<datetime>-<vmname>.zip`
![LogZip1](https://user-images.githubusercontent.com/96930989/233914433-e6028bf4-23ce-4022-a32a-2724b0185fcb.png)

![LogZip2](https://user-images.githubusercontent.com/96930989/233914543-fd878455-92d5-48e5-b269-f4f221bcfa20.png)

### 📝（可选）收集事件日志

```powershell
$logs = @("Application", "System", "Operations Manager")
foreach ($log in $logs) {
    $output = "C:\temp\$log.evtx"
    wevtutil epl $log $output
}
```

---

## 🛡️ 3. Defender 日志
