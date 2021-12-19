# 6 Logging

Logging, is to know, what happened. 记录日志最大的问题在于，处理不同的类型，换句话说，格式化输出。

使用 submodule 来引入第三方库。

```git
git submodule add https://github.com/gabime/spdlog myHazel/vendor/spdlog
```

`std::shared_ptr` 需要 `#include <memory>`。在类里的 static function 是可以在没有实例的情况下被调用的，那么如果此时返回一个类的属性的话，这个属性也得是 static 的。

在正在开发的库里面，新建一个类，来封装第三方库 spdlog，这样 CLIENT 就不用知道第三方库的细节。

使用 `#define` macro 来简化常用代码

使用 macro 还有一个方便的地方，可以检测是否是 distribute build，如果是的话，可以把 macro 定义为空，这样就不会在发行版中因输出log导致运行速度变低了。

这一节没有理解的地方是，Log.cpp 中为什么还要声明 `s_CoreLogger` 和 `s_ClientLogger`。这俩在头文件里面已经有了啊。姑且认为是为了方便在上下文中理解代码，所以挪过来了一份。