# 9 Event System

Key pressed event vs. key repeated event demo

因为作者在这个视频开始引入了新的教学方法，不再现场敲代码了，而是复制已有的代码，然后讲解。所以我也不再完全从零开始在自己的仓库里面开发并学习，而是克隆的作者的仓库，`checkout` 到了相应的 `commit` 。

相应的更改

1. `GenerateProjects.bat` 里改成了 `vs2019`
2. `premake5.lua` 里， `systemversion` 改成了 `latest`。

但是遇到问题了。

```
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Microsoft\VC\v160\Microsoft.CppCommon.targets(155,5): error MSB3073: The command "IF EXIST ..\bin\Debug-windows-x86_64\Hazel\Hazel.dll\ (xcopy /Q /E /Y /I ..\bin\Debug-windows-x86_64\Hazel\Hazel.dll ..\bin\Debug-windows-x86_64\Sandbox > nul) ELSE (xcopy /Q /Y /I ..\bin\Debug-windows-x86_64\Hazel\Hazel.dll ..\bin\Debug-windows-x86_64\Sandbox > nul)
```

看上去跟拷贝 `Hazel.dll` 有关。打开相关目录之后，发现拷贝未完成，手动拷贝后，`Sandbox.exe` 是可以运行的。

但是命令行运行下面这段脚本是可以的。
```bat
D:\vs_proj\Hazel\Hazel>xcopy /Q /Y /I ..\bin\Debug-windows-x86_64\Hazel\Hazel.dll ..\bin\Debug-windows-x86_64\Sandbox > nul

D:\vs_proj\Hazel\Hazel>
```

[查阅](https://stackoverflow.com/questions/17075279/how-do-i-fix-msb3073-error-in-my-post-build-event)之后，意识到是这个拷贝指令放置的位置的问题，该指令放在了 `Hazel` 项目的 `post-build event` 里面，而此时还没有 `Sandbox` 文件夹，所以上述命令是运行不成功的。

再按一次 `Rebuild` 就可以了，因为此时已经有了 `Sandbox` 文件夹。

更好的方案是，修改 `premake5.lua` 里的

```lua
postbuildcommands
		{
			("{COPY} %{cfg.buildtarget.relpath} ../bin/" .. outputdir .. "/Sandbox")
		}
```

这段话，到对应的 `Sandbox` 项目下面，但是此时不能再使用 `relpath` macro（或者说我并不知道应该怎么引用到另一个项目的 `buildtarget` ）。

