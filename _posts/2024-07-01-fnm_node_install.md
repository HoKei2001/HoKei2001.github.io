---
title: fnm and nodejs install
date: 2024-07-01 12:00:00 +0000
categories: [Crawler, Crawlee]
tags: [node.js, crawlee,fnm]     # TAG names should always be lowercase
---


# fnm installation

## Why writing this
1. 项目涉及到爬虫
2. Crawlee 一个非常优秀的开源爬虫框架，这玩意是用js和ts写的，考虑写js脚本然后再集成到python项目
3. Setting up需要  pre-requisites：
   1. Have Node.js version 16.0 or higher installed.
   2. Have NPM installed, or use other package manager of your choice.
4. 推荐fnm作为nodejs的版本管理工具

## fnm install and config
1. Go to [fnm github](https://github.com/Schniz/fnm)
2. Download the latest reslease
   - Here I download `fnm-windows.zip`
   - ![alt text](/assets/img/2024-07-01-fnm_node_install/image.png)
3. Extract and get the `fnm.exe`   
   - ![alt text](/assets/img/2024-07-01-fnm_node_install/image1.png)
   - Add env PATH and then test
   - `$ fnm --version`
   - `fnm 1.37.1`
4. ps1 settings
   1. Check path ![alt text](/assets/img/2024-07-01-fnm_node_install/image2.png)
   2. Go to create that file ![alt text](/assets/img/2024-07-01-fnm_node_install/image3.png)
   3. Write this `fnm env --use-on-cd | Out-String | Invoke-Expression` and save.
   4. See some errors when restarting PowerShell, and then [Follow this](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4) just input:
    `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    `
   5. Restart and no error means OK
5. node.js install
   1. `fnm list-remote`:查看可以安装的node.js版本列表
   2. `fnm list`: 查看已安装的nodejs版本
   3. `fnm-env`:配置安装目录/下载镜像等
      1. 创建名为`FNM_DIR`的变量，路径为自定义的安装目录
      2. ![alt text](/assets/img/2024-07-01-fnm_node_install/image5.png)
   4. `fnm install 20`: 安装v20 nodejs
   5. `node -v`:检查nodejs安装版本![alt text](/assets/img/2024-07-01-fnm_node_install/image4.png)
   6. 添加npm路径为环境变量
      1. ![alt text](/assets/img/2024-07-01-fnm_node_install/image7.png)
      2. ![alt text](/assets/img/2024-07-01-fnm_node_install/image6.png)
      3. `npm -v`:Check the npm version

##  Summary
For now we alr installed the nodejs and its version manager fnm successfully. Then just start to install crawlee.




