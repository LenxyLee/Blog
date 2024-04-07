---
title: TBSandbox-Day2.5
date: 2024-04-05 21:35:29
tags: 沙箱分析
---

# 微步沙箱分析-Day2.5

> 这是Day2.5 用来总结一些奇淫艺技用的

之前有人说可以使用检查QQ内存大小来判断是否为微步沙箱

于是我们来测试下看看 糊了一个程序

```rust
use std::process::Command;
use sysinfo::{System};
fn main() {
    println!("Hello, world!");
    let s = System::new_all();
    for (_pid,process) in s.processes(){
        let process_name = process.name();
        if process_name == "QQ.exe" {
            println!("Process name is {} Memory is {}",process_name,process.memory())
        }
    }
    let _ = Command::new("cmd.exe").arg("/c").arg("pause").status();
}

```

![](../img/TBSandbox-Day2-5/image-20240405222531691.png)

结果是可行的，QQ只存在单进程且仅有2MB.且通过这个方法似乎可以使微步执行超时？

## 来看看样本吧  未完待续（ 打群星去了

![](../img/TBSandbox-Day2-5/image-20240406161627494.png)

也不知道是不是上次那哥们传的 就用这个分析以下看看吧

### 反调试

```c
  if ( IsDebuggerPresent() )   //检测当前进程是否正在被调试器调试
    goto LABEL_2;
  pbDebuggerPresent = 0;
  CurrentProcess = GetCurrentProcess();
  if ( CheckRemoteDebuggerPresent(CurrentProcess, &pbDebuggerPresent) )   //检测是否正在被远程调试器调试
  {
    if ( pbDebuggerPresent )
      goto LABEL_2;
  }
```

糊一个调试器的测试代码吧 远程调试器就不写了

```rust
extern crate winapi;

use winapi::um::winuser::IsDebuggerPresent;

fn main() {
    let debugger_present = unsafe { IsDebuggerPresent() };
    if debugger_present != 0 {
        println!("Debugger is present.");
    } else {
        println!("Debugger is not present.");
    }
}
```

### 判断是否为远程链接

```c
  if ( GetSystemMetrics(4096) )
    goto LABEL_2;
```

通过**GetSystemMetrics(SM_REMOTESESSION)**来判断是否为远程桌面.

```rust
extern crate winapi;

use winapi::um::winuser::GetSystemMetrics;

fn main() {
    let remote_session = unsafe { GetSystemMetrics(4096) };
    if remote_session != 0 {
        println!("Current session is a remote session.");
    } else {
        println!("Current session is not a remote session.");
    }
}
```



![](../img/TBSandbox-Day2-5/image-20240406163153287.png)
