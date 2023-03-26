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



## 2. Capture tracing logs in Fiddler
#### Install the [Fiddler Classic tool](http://www.telerik.com/download/fiddler). This tool will detect the traffic from your computer to the external websites.
![image](https://user-images.githubusercontent.com/96930989/227168245-119b143e-cb3a-4f1e-b333-679b1c2b7d23.png)

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
