# Windows PowerShell功能记录

[查看官方文档](http://www.powershelladmin.com/wiki/SSH_from_PowerShell_using_the_SSH.NET_library)

## 一、Windows PowerShell链接Linux

### 1、导入SSH模块

    Import-Module SSH-Sessions

### 2、创建一个到Linux主机的Session（会话）

    New-SshSession -ComputerName 172.32.3.201 -Username jiaxun

### 3、查看Session（会话）

    Get-SshSession

### 4、登录Session（会话）

    Enter-SshSession 172.32.3.201

### 5、删除Session（会话）

    Remove-SshSession 172.32.3.201

### 6、操作Linux

    $Query = Invoke-SshCommand -ComputerName 172.32.3.201 -Command "sudo rm yangfan"

### 7、帮助操作

    Get-Help "ssh"
    Get-Help new-SshSession