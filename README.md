



## 使用说明（英文）

[XiebroC2-v3.1-README](https://github.com/INotGreen/XiebroC2/blob/main/README_EN.md)

## 更新

[+] 2024.2.18 XiebroC2-3.1发布

[+] 2024.2.20 XiebroC2-3.1 修复bug： [xiebroc2](https://github.com/INotGreen/XiebroC2/releases/download/XieBroC2-v3.1/XiebroC2-v3.1.7z)

[+] 2024.2.29 XiebroC2更新，添加Websocket通信协议，支持域前置和cdn上线、修复3.1中的数个bug:[xiebroc2-v3.1.1](https://github.com/INotGreen/XiebroC2/releases/download/XiebroC2-v3.1.1/XiebroC2-v3.1.1.7z)

[+] 2024.3.7 



随缘更新中。。。

如果您喜欢该项目的话，可以右上角star + fork + follow，非常感谢！

## 特点

- 被控端(Client)由Golang编写，兼容WIndows、Linux、MacOS上线（未来会考虑移动端上线）

- 团队服务器(Teamserver)由.net 8.0 编写、AOT编译，内存占用低，无需安装任何依赖，几乎可以兼容全平台系统

- 控制端(Controller)支持反弹shell，文件管理、进程管理、网络流量监控、内存加载、自定义UI背景色等功能

- 支持Windows/Linux内存加载PE文件，即文件不落地执行木马，中转第三方C2/RAT

- 支持内存执行.net 程序集（execute-assembly、inline-assembly)

- 支持通过lua扩展UI控件、Session命令，载荷生成（类似于CobaltStrike的cna脚本）

- 支持自定义RDIshellcode（仅64位，32位需要手动编译client）或者用[donut](https://github.com/TheWover/donut)、[Godonut](https://github.com/Binject/go-donut)生成属于自己的shellcode

- 支持Teamserver托管二进制文件、文本、图片(类似SimpleHttpServer)

- 支持团队服务器自定义配置文件,自定义Telegram的chat ID/Token上线通知

- 控制端(Controller)UI轻量级交互界面，内存占用大约是CobaltStrike的60分之一，是Metasploit的10分之一

- Golang的编译器特征已经被部分AV/EDR厂商标黑了,因此免杀效果较差

  

  

## 支持平台

**Client(Session)**

|    Windows（x86_x64）    | Linux(x86_x64) | MacOS |
| :----------------------: | :------------: | :---: |
|        Windows11         |     ubuntu     | AMD64 |
|        Windows10         |     Debian     | i386  |
|       Windows8/8.1       |     CentOS     |  M1   |
|         Windows7         |    ppc64le     |  M2   |
|        Windows-XP        |      mips      |       |
| Windows Server 2000-2022 |     s390x      |       |



## 快速使用

- 无需安装任何环境直接使用：[XiebroC2](https://github.com/INotGreen/XiebroC2/releases)

- 修改TeamServerIP和TeamServerPort为VPS的IP和端口，然后保存为profile.json

- 需要解释的几个参数：

  1.如果配置文件（profile.json）中的Fork为true，则在执行runpe和nps的时候会创建一个子进程，并且进行注入，这就是传统的Fork&&Run模式，如果Fork为flase，则为内联（inline）加载模式，如果在有严格AV/EDR的环境下，内联加载的方式显然更加的OPSEC

  2.如果你需要上线Telegram提醒，您还需要去学习，TG的机器人消息提醒，在配置文件中填上Telegram_Token和Telegram_chat_ID

  3.shellcode目前只支持x64位，原谅我的懒惰，我觉得x86环境是比较罕见需求，因此在这里就忽略掉了，至于rdiShellcode64是怎么生成的，您需要仔细地研究[sRDI](https://github.com/monoxgas/sRDI)的原理，这并不是一句话能说清楚的

  4.windows\Session\Reverse_tcp不支持域前置，您需要添加windows\Session\Reverse_Ws监听器才能使用域前置和cdn上线。

  在配置文件（profile.json）中 可以自定义Route来修改websocket路由的特征。

```json
{
    "TeamServerIP": "192.168.1.250",
    "TeamServerPort": "8880",
    "Password": "123456",
    "StagerPort": "4050",
    "Telegram_Token": "",
    "Telegram_chat_ID": "",
    "Fork": false,
    "Route": "www",
    "Process64": "C:\\windows\\system32\\notepad.exe",
    "Process86": "C:\\Windows\\SysWOW64\\notepad.exe",
    "WebServers": [],
    "listeners": [],
    "rdiShellcode32": "",
    "rdiShellcode64": "",
}
```

服务端：

```bash
Teamserver.exe -c profile.json
```



## 上线演示

<video src="https://private-user-images.githubusercontent.com/89376703/305162512-771c2e88-afd8-493d-a575-7e10149837dd.mp4" width="640" height="480" controls></video>





## 命令列表




| Commands         |               Usage                |                   Description                    |
| :--------------- | :--------------------------------: | :----------------------------------------------: |
| nps              |     nps  “powershell command”      |        Unmanaged run powershell in memory        |
| Inline-assembly  | inline-assembly  “FilePath” “args” |           Inline execute .net assembly           |
| execute-assembly | execute-assembly “FilePath” ”args” | Fork child process execute loader .net  assembly |
| runpe            |      runpe  “FilePath” “args”      |    loader C/C++ PE in the memory for Windwos     |
| shell            |        shell “cmd command”         |                 Execute  command                 |
| powershell       |  powershell “powershell command”   |            Execute powershell command            |
| checkAV          |              checkAV               |             Detect AV/EDR processes              |
| upload           |   upload “RemotePath” “FilePath”   |            Upload File to the target             |
| memfd            |      memfd “FilePath” “args”       |        PE loader in the memory for Linux         |
| help             |                help                |                View command list                 |
| cls              |                cls                 |                   Clear screen                   |





## 添加插件

<video src="https://private-user-images.githubusercontent.com/89376703/305687743-fb39df88-0f29-4359-9cd4-fc4bfa698270.mp4" width="640" height="480" controls></video>

## 拖拽式批量上传文件

<video src="https://private-user-images.githubusercontent.com/89376703/306153487-551e96db-9253-4a9f-8c2d-5c99c0280c8a.mp4" width="640" height="480" controls></video>



## 插件编写

- 学习编写lua插件:[Xiebro-Plugins](https://github.com/INotGreen/Xiebro-Plugins?tab=readme-ov-file#executeassembly)



## TODO

- 目前正反向代理和端口转发未开放，未来考虑完善和开发这个功能。
- 目前仅支持TCP/WebSocket协议的Session模式，它们是https的代替品，后续考虑开发可靠的UDP协议并且支持Beacon模式
- 考虑开发Powershell、VBscript、Hta、Jscript等载荷。
- 开放更多窗体和API接口，以便lua扩展插件



## 免责声明

本项目仅用于渗透测试演练的学习交流和研究，强烈不建议您用于任何的实际途径（包括黑灰产交易、非法渗透攻击、割韭菜），网络不是法外之地！如果您使用该工具则应该自觉遵守以上要求。

为了避免该工具被非法分子利用，所以本人已经将危害较大的功能删除，只留下部分功能作为渗透测试演练demo，teamserver和Controller不进行开源

## 提交Bug和建议



<img src="Image\Image.jpg"  />



