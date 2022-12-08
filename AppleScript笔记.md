## 在 AppleScript 中打开 terminal 并执行 shell 脚本

```applescript
tell application "Terminal"
    do script "/opt/homebrew/opt/postgresql@15/bin/postgres -D /opt/homebrew/var/postgresql@15"
		do script "php /Users/tom/laravel_project/artisan serve"
end tell
```

上面这个脚本运行后，会先后开启2个 terminal 窗口，其中一个启动了 postgresql 的服务，另一个开启了 Laravel 内置的 HTTP server 。

参考：

* https://stackoverflow.com/questions/1870270/sending-commands-and-strings-to-terminal-app-with-applescript
* https://stackoverflow.com/questions/10022161/open-programs-with-applescript
* https://developer.apple.com/library/archive/documentation/LanguagesUtilities/Conceptual/MacAutomationScriptingGuide/CallCommandLineUtilities.html

## 在 AppleScript 中打开 Safari 并访问特定页面

```applescript
tell application "Safari"
    open location "https://github.com/"
    activate
end tell
```

## 获取 Finder 窗口所在的目录并在 terminal 中进入对应的目录

```applescript
tell application "Finder" to set the myPath to (the target of the front window) as alias
set myPath to POSIX path of myPath

tell application "Terminal"
    activate
    do script ("cd " & quoted form of myPath)
end tell
```
