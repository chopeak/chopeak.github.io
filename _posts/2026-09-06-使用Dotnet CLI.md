---
layout: post
title: 使用Dotnet CLI
tags: [CSharp,Dotnet CLI]
---

##### 使用Dotnet CLI(Dotnet命令行接口Command-Line Interface)

```powershell
# 1.打开终端PowerShell,创建并进入文件夹HelloWorld
mkdir HelloWorld && cd HelloWorld
# 2.执行Dotnet new console命令生成程序基架
dotnet new console
# 3.运行生成的程序dotnet run(运行此命令时,会执行dotnet build)
dotnet run
# 4.编辑Program.cs文件
vim Program.cs
# 5.再次执行dotnet run
dotnet run
```

