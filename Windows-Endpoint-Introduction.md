# Windows Endpoint Introduction

## Windows Processes

A process is an instance of a computer program that is running in system memory, used by both the OS and applications.

The OS also uses processes, but in this context, they are referred to as services. `Windows Services` are processes run with minimal user interaction, often launched during boot.

Services are monitored for their `Status` as well as their `Startup` Type.A service can have a status, such as "Running" or "Stopped". Startup Type indicates when the service is to be started.

Common Startup Types include, but are not limited to:

- Manual - to be run by the user.
- Automatic - to be executed at boot time.
- Disabled - to not execute, even when requested by the system or user.

We can run `services.msc` to list all services, their status, and their startup type.

## Windows Registry

Windows maintains service and application configurations in the `Windows Registry`.

The registry stores settings, options, and other miscellaneous information in a hierarchical tree structure of `hives`, `keys` and `values`.

**Keys** are the defining data structure for the registry. A key can contain single values related to a necessary configuration or additional keys possibly containing more values or keys.

The values are comprised of three fields: 
- **name** : A meaningful description of the value, such as `Load_On_Startup`, which indicates that an application is to be started when the system boots.
- **type** : Represents the format of the value such as `REG_SZ` for a string or `REG_DWORD` for an unsigned integer.
- **data** : The actual value conforming to the type.

## Command Prompt

The Windows `command prompt` is the most commonly-used command-line interface for the operating system. It is also known as cmd.exe, which is the name of the executable.

Systems administrators and users can automate command-line tasks with `batch files`, which store a series of commands and, when executed, will run them in sequence.

> Note: Windows batch files are an excellent choice in terms of running system commands, but they don’t manage error handling very well and they aren’t modular.

**For Example**;

```
@ECHO OFF
TITLE Example Batch File
ECHO This batchfile will show Windows 10 Operating System information
systeminfo | findstr /C:"Host Name" systeminfo | findstr /C:"OS Name"
systeminfo | findstr /C:"OS Version" systeminfo | findstr /C:"System Type"
systeminfo | findstr /C:"Registered Owner"
PAUSE
```
**Explaination**;

|Command|Description|
|---|---|
|@ECHO OFF|Turns off echoing of commands in the batch file.|
|TITLE Example Batch File|Sets the title of the command prompt window.|
|ECHO This batchfile will show Windows 10 Operating System information|Displays a message in the command prompt.|
|systeminfo | findstr /C:"Host Name"|Retrieves the host name of the system.|
|systeminfo | findstr /C:"OS Name"|Retrieves the name of the operating system.|
|systeminfo | findstr /C:"OS Version"|Retrieves the version of the operating system.|
|systeminfo | findstr /C:"System Type"|Retrieves the system type (32-bit or 64-bit).|
|systeminfo | findstr /C:"Registered Owner"|Retrieves the registered owner of the system.|
|PAUSE|Pauses execution until a key is pressed.|

## Visual Basic Script (VBScript) 

It is a robust scripting language that could leverage programming features such as-

- functions
- file I/O
- network connectivity

These scripts require a special file extension (.vbs), but must be run through the cscript.exe interpreter.

**For Example**;

```
strComputer = "."
Set objWMIService = GetObject("winmgmts:" _
	& "{impersonationLevel=impersonate}!\\" & strComputer & "\root\cimv2")

Set colOSes = objWMIService.ExecQuery("Select * from Win32_OperatingSystem")
For Each objOS in colOSes
	Wscript.Echo "Computer Name: " & objOS.CSName
	Wscript.Echo "Caption: " & objOS.Caption 'Name
	Wscript.Echo "Version: " & objOS.Version 'Version & build
	Wscript.Echo "Build Number: " & objOS.BuildNumber 'Build
	Wscript.Echo "Build Type: " & objOS.BuildType
	Wscript.Echo "OS Type: " & objOS.OSType
	Wscript.Echo "Other Type Description: " & objOS.OtherTypeDescription
	WScript.Echo "Service Pack: " & objOS.ServicePackMajorVersion & "." & _
		objOS.ServicePackMinorVersion
Next 
```

**Explaination**;

|Line|Description|
|---|---|
|strComputer = "."|This line assigns the value of the string "." to the variable `strComputer`. This dot represents the local computer.|
|Set objWMIService = GetObject("winmgmts:" _ & "{impersonationLevel=impersonate}!\\" & strComputer & "\root\cimv2")|This line sets the `objWMIService` variable to access the Windows Management Instrumentation (WMI) service on the local computer. It connects to the CIMv2 namespace, which contains classes for managing operating system and hardware components.|
|Set colOSes = objWMIService.ExecQuery("Select * from Win32_OperatingSystem")|This line executes a WMI query to retrieve information about the operating system. It selects all properties (`*`) from the `Win32_OperatingSystem` class. The result is stored in the `colOSes` collection.|
|For Each objOS in colOSes|This line begins a loop that iterates through each operating system object (`objOS`) in the `colOSes` collection.|
|Wscript.Echo "Computer Name: " & objOS.CSName|This line prints the computer name (hostname) of the operating system using the `CSName` property of the `objOS` object.|
|Wscript.Echo "Caption: " & objOS.Caption	|This line prints the name or caption of the operating system using the `Caption` property of the `objOS` object.|
|Wscript.Echo "Version: " & objOS.Version	|This line prints the version of the operating system using the `Version` property of the `objOS` object.|
|Wscript.Echo "Build Number: " & objOS.BuildNumber|This line prints the build number of the operating system using the `BuildNumber` property of the `objOS` object.|
|Wscript.Echo "Build Type: " & objOS.BuildType|This line prints the type of build of the operating system using the `BuildType` property of the `objOS` object.|
|Wscript.Echo "OS Type: " & objOS.OSType|This line prints the type of operating system using the `OSType` property of the `objOS` object.|
|Wscript.Echo "Other Type Description: " & objOS.OtherTypeDescription|This line prints additional description about the type of operating system using the `OtherTypeDescription` property of the `objOS` object.|
|WScript.Echo "Service Pack: " & objOS.ServicePackMajorVersion & "." & objOS.ServicePackMinorVersion|This line prints the service pack version of the operating system using the `ServicePackMajorVersion` and `ServicePackMinorVersion` properties of the `objOS` object.|

## PowerShell

PowerShell’s versatility for end users and administrators alike has allowed it to replace cmd.exe as the Window CLI of choice.

PowerShell scripts, like VBScript files, are plaintext files. Although we could create them with a text editor, we can use the built-in Powershell `Integrated Scripting Environment` (ISE) to write, run, and monitor our code in real-time.

PowerShell’s execution policy is a protective measure designed to block potentially malicious scripts. Before using PowerShell, we should check which policy is configured on our system.

- To check our execution policy-

```
Get-ExecutionPolicy
```

![image](https://github.com/vsang181/Security-Operations-and-Defensive-Analysis/assets/28651683/c8baaf4e-bfc2-43f6-9bec-de5f256a2ef9)

**Example**;

```
Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object - Property CSName, Caption, Version,BuildNumber, BuildType, OSType, RegisteredUser, OSArchitecture, ServicePackMajorVersion, ServicePackMinorVersion
```

|Command Part|Explanation|
|---|---|
|Get-CimInstance|This cmdlet retrieves instances of a CIM (Common Information Model) class. In this case, it retrieves instances of the `Win32_OperatingSystem` class, which contains information about the operating system.|
|-ClassName Win32_OperatingSystem|This specifies the class name from which to retrieve instances, in this case, `Win32_OperatingSystem`.|
|Select-Object|This cmdlet selects specific properties of the objects passed through the pipeline.|
|-Property|This parameter specifies the properties to include in the output.|
|CSName, Caption, Version, BuildNumber, BuildType, OSType, RegisteredUser, OSArchitecture, ServicePackMajorVersion, ServicePackMinorVersion|These are the properties of the `Win32_OperatingSystem` class that are selected to be included in the output. They represent various pieces of information about the operating system such as the computer name, operating system name, version, build number, build type, OS type, registered user, OS architecture, service pack major version, and service pack minor version.|

-  To list running services-

```
Get-Service
```

- To list only running services-

```
Get-Service | Where-Object { $_.Status -eq "Running" }
```

- To load full documentation for PowerShell cmdlets-

```
Get-Help {cmdlet}
```

Aliases in PowerShell are alternativenames for cmdlets that may be easier to remember or understand.

- To get alias for PowerShell cmdlets-

```
Get-Alias {cmdlet}
```

## Programming on Windows

Windows command-line applications are differentiated by short (1-3 character) file extensions that dictate the format of the program.

When Windows-based code is compiled, it is stored in a `portable executable` format.

The most recognized extensions for compiled, executable code in Windows are `.exe` and `.dll`, which designate `executable` and `dynamic-link library` files. Both exe and dll files are part of the portable executable (PE) format.

### Component Object Model

COM provided a versatility that was compatible with numerous programming languages as a code wrapper. Code wrappers can reduce complexity of code without sacrificing the utility of the codebase within them.

COM also made interprocess communication more streamlined for the average Windows user’s productivity applications. For example, embedded Excel spreadsheets in Word documents could be updated from Excel, and those changes would be reflected in the embedded object in the Word document. This interconnectivity of programs is a common user experience of interprocess communication.

As Windows-based computers became networked, the COM was upgraded resulting in the `Distributed Component Object Model (DCOM)`.

As users began surfing the internet with web browsers, another COM-like framework was introduced in 1996, `ActiveX`.

### .NET and .NET Core

Microsoft began a significant development platform modernization in the early 2000s with the .NET Framework. .NET introduced the new programming languages C# and Visual Basic.NET, which provide wrappers for the Windows API as well as COM objects within the operating system. 

The framework also incorporated secure coding mechanisms, and can differentiate between locally-developed code and Internet-downloaded code. 

It can also determine whether code execution requires elevated privileges and verify if these privileges have been granted to the application. 

## Windows Event Log

.
.
.
.
.
.
_In progress..._

## Let's Connect

I welcome your insights, feedback, and opportunities for collaboration. Together, we can make the digital world safer, one challenge at a time.

- **LinkedIn**: (https://www.linkedin.com/in/aashwadhaama/)

I look forward to connecting with fellow cybersecurity enthusiasts and professionals to share knowledge and work together towards a more secure digital environment.
