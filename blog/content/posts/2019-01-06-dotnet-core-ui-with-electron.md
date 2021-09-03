---
title: 'Writing .NET Core Desktop Apps with Electron.NET'
date: 2019-01-06T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2019/01/dotnet-core-ui-with-electron
categories:
  - development
tags:
  - programming
  - software
  - development
  - csharp
  - microsoft
---

The team behind GitHub has created its own interesting open source projects in addition to the web platform. One of them is Electron, a cross-platform desktop solution used for the Atom editor. Microsoft also uses Electron for Visual Studio Code.

Electron runs with NodeJS, so applications for it are usually written in JavaScript. For .NET developers there is now [Electron.NET](https://github.com/ElectronNET/Electron.NET) available, an Electron wrapper for ASP.NET Core applications.

An Electron.NET application hosts the ASP.NET Core project. Electron.NET then provides access to Electron via API. This enables platform-specific functionalities such as push notifications or clipboard access. And the best thing about it is that you don't notice anything about the underlying JavaScript.

Let's start right away and develop a simple application with Electron.NET. You need first the .NET Core 2 SDK, as well as NodeJS, at least in version 8.6.0. I use Visual Studio 2017 as development environment.  I start by creating an empty ASP.NET Core project. In the following dialog I choose the MVC template. And I'm ready to go.

First I install Electron.NET using [NuGet](https://www.nuget.org/packages/ElectronNET.API/). After that the WebHostBuilder in Program.cs has to be adjusted as follows:

```csharp 
public class Program
{ 
  public static void Main(string[] args) 
  { 
    CreateWebHostBuilder(args).Build().Run(); 
  } 

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) => 
    WebHost.CreateDefaultBuilder(args) 
    .UseElectron(args) 
    .UseStartup<Startup>(); 
} 
```

The application has now been extended with Electron, but so far you don't see anything of it. If you start the project now, it will be started in the browser as usual. However, I would like the application to be started in the familiar Electron window as it would be displayed in the browser. To achieve this, I add the following line at the end of the Configure() method: 

```csharp 
Task.Run(async () => await Electron.WindowManager.CreateWindowAsync()); 
```

If you want to be able to start your application as a classic web application, you have to put this statement in an if statement with the condition *HybridSupport.IsElectronActive*.

Now another Nuget package is needed, which is not installed for the project itself, but as a global toolset. Use the following command in the Nuget console:

```xml 
dotnet tool install ElectronNET.CLI -g 
```

This package provides several tools that you can use, among other things, to initialize and start your project. Then navigate to the project directory and execute the command "electronize init". The command creates a configuration file called electron.manifest.json. Now you can start your application from here with the command "electronize start". The first start takes a while, but this does not apply to the following starts. At this point you should be able to see your ASP.NET Core application in the normal Electron interface. So let's try some of the features now.

A quick tip at this point: To debug the application you can simply select the menu item "Debugging" in Visual Studio and then select the application in the "Attach to Process" area. You can also use the shortcut CTRL + SHIFT + I to access the Chrome Developer Tools. This functionality is provided via Electron.NET CLI, however, if you want to implement such functionality for later use, you can do it yourself: Electron.WindowManager.BrowserWindows.First().WebContents.OpenDevTools().

Electron creates multiple processes. The main process called Main contains the code of Startup.cs and Program.cs. A separate renderer process is created for each desktop window, which prevents the entire application from crashing when a renderer process crashes. The communication between these processes is based on Inter-Process Communication (IPC).

The classic workflow for ASP.NET Core MVC is as follows: The user makes an input and an action command is sent to a controller. The controller returns a result in the form of a view instance, which has a disadvantage: The page is reloaded, which doesn't provide great user experience. With the help of IPC, however, it is easy to remedy the situation:

I add code to my Index.cshtml file that uses JS to access the API of the renderer process. The ipcRenderer allows message-based communication, optional arguments can be passed.

```html 
<h3 id="header"></h3> 
<button id="cmdShowData">Show Data</button> 

<script> 
  (function () { 
    const { ipcRenderer } = require("electron");

    document.getElementById("cmdShowData").addEventListener("click", () => { 
      ipcRenderer.send("getData", 'args'); 
    }); 

    ipcRenderer.on('sendData', (event, msg) => { 
      document.getElementById("header").innerHTML = msg; 
    });
  }()); 
</script> 
```
 
I also need a possibility to listen to incoming messages. This is implemented inside the controller (HomeController.cs): 

```csharp 
public IActionResult Index()
{ 
  Electron.IpcMain.On("getData", (args) => 
  { 
    var mainWindow = Electron.WindowManager.BrowserWindows.First(); 
    Electron.IpcMain.Send(mainWindow, "sendData", "Hello IPC World!"); 
  }); 

  return View(); 
} 
```

As you can see, I am only returning a simple message, but of course you can extend this or implement advanced functionality like repeated execution and update of the message by loops.

I don't want to go into much application development at this point, but you can see how quickly you can package an ASP.NET core application in Electron. But there is one important detail missing for developing real applications: Deployment. In fact, two applications are created when the Electron.NET app is being built. On the one hand, the ASP.NET core application is built including .NET Core runtime. This application is then packaged by Electron Packer like a regular Electron application. You can specify the target operating system by adding the relevant target flag to the build command: Use the command electronize build /target win. Use the flag osx for macOS, or linux for Linux. After execution, you have your application packaged and ready for use. 