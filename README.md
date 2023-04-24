# Capture different network tracing logs

## 1. Capture HAR logs in Edge/Chrome

#### 1. Launch the Edge/chrome browser
#### 2. From the menu, select `... -> More tools -> Developer Tools`.(or press `F12` directly)
#### 3. Select the `Network` tab
![image](https://user-images.githubusercontent.com/96930989/212235788-5cf820a0-a909-44e9-8a8f-e39f1c7a9df6.png)
#### 4. Make sure "Preserve Log" is checked
![image](https://user-images.githubusercontent.com/96930989/212235841-de012d95-7602-45c5-8561-e4d817fd1507.png)
#### 5. Select the concentric circle icon (Or use the keyboard shortcut CTRL+ E) to start recording the session.  

This will turn the button red and replace the filled inner circle with a `filled square`.

Select the same filled square button to stop recording the session
![image](https://user-images.githubusercontent.com/96930989/212235902-68a27a79-9ed0-4e25-855f-545190ced37e.png)

#### 6. Select the `"Export HAR..."` button to export the recorded session to a HAR file. The button looks like an arrow pointing downwards to a horizontal line. You can then save the log to your local machine.
![image](https://user-images.githubusercontent.com/96930989/212235965-a384ebf9-63a9-441a-9f64-9f83ca58c5ad.png)



## 2. Capture tracing logs in Fiddler (Windows)
#### Install the [Fiddler Classic tool](http://www.telerik.com/download/fiddler). This tool will detect the traffic from your computer to the external websites.
![image](https://user-images.githubusercontent.com/96930989/227784190-16f3cb11-8822-4b3d-bfcd-09bd6ba29b1e.png)

#### Once you install the fiddler you need to perform the below-mentioned steps:
##### 1. Go to START button
##### 2. Click on `Fiddler 4` 
##### 3. Click on `Tools > Options`
![image](https://user-images.githubusercontent.com/96930989/227168418-ea792ab4-10cd-49de-93cf-cafd47fefa3b.png)
##### 4. Click `HTTPS` Tab        
##### 5. Check the Box Against `Capture HTTPS CONNECTs and Decrypt HTTPS traffic`
![image](https://user-images.githubusercontent.com/96930989/227168518-b7ccd9e7-a969-46b9-a696-d94a32b1dc4e.png)

##### 6. Click on `YES`
![image](https://user-images.githubusercontent.com/96930989/227168593-04f7fee2-562b-471e-9216-17c62915828f.png)

##### 7. Click on `YES`   
![image](https://user-images.githubusercontent.com/96930989/227168640-8fc2d7fb-9825-4706-901b-2f4b21dab3b0.png)

##### 8. Click on `protocols` and add tls1.1 and tls1.2
![image](https://user-images.githubusercontent.com/96930989/227168699-3786939b-8168-47e0-9389-c0bd3cd39962.png)

![image](https://user-images.githubusercontent.com/96930989/227168717-e3d0886c-4269-4358-b943-7b067bf6c15b.png)

##### 9. Reproduce the issue and capture the behavior two or three times
##### 10. Once done, click on File-> Save->Save All session-> Give it a Name and save it.

## 3. Capture tracing logs in network monitor

# Capture local logs from Windows machine

## 1. Capture Windows update logs

Run powershell command
```powershell
Get-WindowsUpdateLog
```
<img width="702" alt="image" src="https://user-images.githubusercontent.com/96930989/233538875-22b3e56b-155f-4b7e-a949-ff8ec066bf37.png">

<img width="964" alt="image" src="https://user-images.githubusercontent.com/96930989/233538942-32e38838-6737-4364-b0f5-6251d092a1ca.png">

<img width="1022" alt="image" src="https://user-images.githubusercontent.com/96930989/233539001-05fe603d-4097-43b6-8c63-994b36adeb5b.png">

## 2. Capture Arc machine related logs

Create folder `temp` under disk C

Launch cmd as admin

run command
```sh
cd "c:\temp"
```

```sh
azcmagent logs --full
```

This will gather log files from the following locations

```
%ProgramData%\AzureConnectedMachineAgent\Log\
%ProgramData%\GuestConfig\arc_policy_logs\
%ProgramData%\GuestConfig\ext_mgr_logs\
%ProgramData%\GuestConfig\extension_logs\
%ProgramData%\GuestConfig\extension_reports\
C:\Packages\Plugins\
```

The logs will be sent to path `c:\temp` in a file named 'azcmagent-logs-datetime-vmname.zip'

![image](https://user-images.githubusercontent.com/96930989/233914433-e6028bf4-23ce-4022-a32a-2724b0185fcb.png)

![image](https://user-images.githubusercontent.com/96930989/233914543-fd878455-92d5-48e5-b269-f4f221bcfa20.png)

We can also additionally gather the following Windows Event Logs if this is a Windows server (and if these Event logs exist - as this is dependent on the extension type being installed).
* Application
* System
* Operations Manager (if the problem is with the Log Analytics/MMA or Dependency agent extensions

## 3. Capture Defender logs


