<div align="center">

# Deploying an ASP.NET Core API to Linux (.NET 6 & Ubuntu 20.04)
</div>

## *This guide assumes you already know how to develop at least a basic .NET-based API and you have an Ubuntu server in place to do the deploying stuff.*

<br/>
<br/>

### **Right, let's jump right in!**

1. **You gonna need to publish your app to a folder, using Visual Studio (or whatever IDE you're using) and navigate to the folder.**

![](https://user-images.githubusercontent.com/46853837/215998278-a5deccd2-85db-4a7a-b05f-474d2f1a9f33.png)

2. **Create a folder in your Ubuntu server *(preferably under `/var/www/`)* and upload your published files to this folder using a tool of your choice.**
   
3. ***(Assuming you're already logged in via ssh, and you don't have .NET SDK installed yet)* Install latest version the .NET SDK using the following commands:**
   * `wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb`<br/>
`sudo dpkg -i packages-microsoft-prod.deb`<br/>
`rm packages-microsoft-prod.deb`
   * `sudo apt-get update && \`<br/>
  `sudo apt-get install -y dotnet-sdk-6.0`<br/>
  You can also refer to [this stuff here](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#2004) for the latest and detailed guide on installing the SDK.

  4. Navigate to your app folder using `cd /var/www/your-app-folder/` and run the following commands to update the file permissions:
   `sudo chmod 755 *`<br/>
   `sudo chown -R $USER:$USER *`