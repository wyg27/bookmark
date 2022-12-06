## 在 AppleScript 中打开 terminal 并执行 shell 脚本

Example

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
