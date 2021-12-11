+++
title = "在 MacOS 上通过 kitty 用 Neovim 打开文件及文件夹"
date = 2021-12-11T11:28:07+08:00
draft = false
indent = false
initial = false
toc = true
keywords = ["MacOS", "kitty", "Neovim", "Automator", "AppleScript"]
description = "在 MacOS 上通过 kitty 用 Neovim 打开文件及文件夹"
+++

- 描述：使用 MacOS 自带的 **自动操作** 就可以很好的完成这个问题。

- 代码如下：

```AppleScript
on run {input, parameters}
    tell application "System Events"
      set isRunning to (exists (processes where name is "kitty"))
    end tell

    if not isRunning then
      tell application "Kitty" to activate
    end if

    set filePath to POSIX path of input

    tell application "/Applications/Kitty.app/Contents/MacOS/kitty"
      activate
      tell application "System Events"
        keystroke "nvim \"" & filePath & "\""
	keystroke return
      end tell
    end tell

end run
```

- 使用流程：

1. MacOS 找到应用程序 `自动操作` ( `Automator` )
2. 新建 `应用程序` ( `Application` )
3. 添加操作 `运行AppleScript`
4. 复制粘贴代码后保存到 `应用程序` ( `Application` ) 文件夹，名字随意。
5. 右键文本文件，选择打开方式，看是否成功

> 注意 ⚠️  在这里可能会提示 不能使用 `System Events` ：
> - 前往 系统偏好设置 -> 安全与隐私 -> 辅助功能 -> 添加你创建保存的应用程序就好

- 附加功能： 右键打开

参考如下链接就好： https://cloud.tencent.com/developer/article/1329530

🤔 就不重复造轮子了知识的轮子了
