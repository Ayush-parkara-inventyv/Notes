# **Low Security**

![](https://miro.medium.com/v2/resize:fit:875/1*Q-4jsGfKbvI-RvVcgNW8EA.png)

**Firstly, go to the GitHub website:** [**https://github.com/artyuum/simple-php-web-shell**](https://github.com/artyuum/simple-php-web-shell)**. From there, download the shell. Then, download the index.php file.**

![](https://miro.medium.com/v2/resize:fit:875/1*FWjw1aAJMkYtrwafisVT-w.png)

**Make sure you first lower the security level on the DVWA site. Then, go to the “Upload” section and press the “Upload” button to upload your file. Choose the downloaded shell file and proceed.**

![](https://miro.medium.com/v2/resize:fit:875/1*_ZHpHYxwCfkSRpKXv7J4Ug.png)

**You will encounter a screen like this. Afterward, copy the part written in red text. Delete the “#” from the URL above and paste the copied text.**

![](https://miro.medium.com/v2/resize:fit:875/1*U6lHIWfdvLjYJRGSv-8U8Q.png)

**If you see something like this, congratulations, you have succeeded. You can execute desired commands from here.**

# **Medium Security**

![](https://miro.medium.com/v2/resize:fit:875/1*I0Jcf4YlClSm4gHWHqobTw.png)

**We have turned the security level back to normal and arrived here.**

![](https://miro.medium.com/v2/resize:fit:875/1*bDWs0PXaifZOwzOGUMAGog.png)

**We open our Burp Suite and enable the intercept. Then, we upload our file.**

![](https://miro.medium.com/v2/resize:fit:875/1*8qq4veBV7D0vTwysdS7PWw.png)

**“When you click ‘Upload,’ you will encounter a screen like this. Right-click on that screen and send it to the repeater.Then, turn off the intercept.”**

![](https://miro.medium.com/v2/resize:fit:875/1*pkgzK0oWX7liTLzPkYnrjA.png)

**“As seen, when we press ‘Send,’ it gives us an error message saying ‘your image was not uploaded.’ From this, we understand that our file needs to be an image.”**

![](https://miro.medium.com/v2/resize:fit:875/1*ThA-cqsBrk0qUFBt49YaGA.png)

**“To fix this, we write ‘image/jpeg’ in the indicated place on the photo. When we do this, the website will treat our PHP file as if it’s an image. Then, we can access the content.**

![](https://miro.medium.com/v2/resize:fit:875/1*0SKNJKFBtQdylZjZsrfC4Q.png)

**“As you can see, the file we uploaded successfully entered the system.”**

![](https://miro.medium.com/v2/resize:fit:875/1*8Umg2BY871kGtoE55TDYPg.png)

**“We find out where our file is uploaded from the Response section. As shown here, then we copy it and paste the URL into our document.”**

![](https://miro.medium.com/v2/resize:fit:875/1*75Lgu0cOZ6nasmj2j6Stvw.png)

**Congratulations!**

# High Level Security

![](https://miro.medium.com/v2/resize:fit:875/1*O6ar7Ki5hinEeKc6fcwA0w.png)

**“Once again, we raise our security level to High and proceed. We uploaded our file again, but we encountered an error once more.”**

![](https://miro.medium.com/v2/resize:fit:875/1*KBg5BKCPewvMCkdgXPIISw.png)

**“This time, we upload any jpg, jpeg, or png file instead of our PHP file. Then, we reactivate the intercept in Burp Suite. We send our result to the repeater again.”**

![](https://miro.medium.com/v2/resize:fit:875/1*vimAY3lRIxeD2j0M6FtMIg.png)

**“This time, we don’t need to deal with the content type. The site doesn’t check it. We change our Content Type to ‘application/x-php.’ After that, when we press ‘Send,’ it gets uploaded.”**

![](https://miro.medium.com/v2/resize:fit:875/1*SDeSBr6_xYdh9lqfTbKMdQ.png)

**“Here, to make the site believe that this is an image, we slightly modify the file name as shown here. This way, the site still thinks that it’s a jpg file.”**

![](https://miro.medium.com/v2/resize:fit:875/1*HFgnLH2aVI5FcDNn9UXvug.png)

**“Next, our jpg file contains certain codes. We delete a few lines from the end of these codes and replace them with the code from our PHP shell file. After that, when we press ‘Send,’ our file gets uploaded.”**

![](https://miro.medium.com/v2/resize:fit:875/1*DiSDdx-Q1YqCFrBSbtNbtw.png)

**“If we also paste the link of the uploaded file it gave us into our URL, our result is that we can now execute the desired command.”**