# [AppleScript 让 Mac 自动化](https://github.com/yeshiqing/Blog/issues/13)

## 基础语法
### 注释
```AppleScript
-- 这是一条注释
# 这也是一条注释
  
(**
  这是多行注释
**)
```

### 调试
```AppleScript
log 123
```

### 对话框
```AppleScript
display dialog "<调试的内容>" -- 注意对象无法转换成字符串，布尔值、数字可以

-- 只带一个按键的 dialogue
display dialog "<弹出框显示的内容>" buttons {"OK"} default button 1

-- 获取用户输入内容。包含 placeholder，default，title，Cancel and Ok buttons
set searchString to text returned of (display dialog "Enter a string to search for:" default answer "" with title "Find Google Chrome Tab")
```

### 字符串只支持双引号，不支持单引号
```AppleScript
tell application "Typora"
end tell
```
在命令行中执行 AppleScript，`osascript -e <script>`后面只能接双引号
```Shell
# 由于外部只能用双引号，里面的双引号都要用转义字符
osascript -e "tell application \"iTerm2\" to set miniaturized of every window to true"
```
参考资料：
[AppleScript 入门：探索 macOS 自动化](https://sspai.com/post/46912)

## 查询应用提供的 API
打开「脚本编辑器」→ Window → Library → 添加应用

## 具体应用场景
### 模拟键盘操作
在指定程序中模拟键盘按键
```AppleScript
tell application "System Events"
    -- 按下 ⌘A
    keystroke "a" using command down
    -- 按下 ⌘V
    keystroke "v" using command down
    -- 按下 ⌘S
    delay 0.1 -- 给油猴脚本代码查错的 UI 事件一个时间
    keystroke "s" using command down
    --按下 ⌘up
    key code 126 using command down
end tell
```
参考资料：
[AppleScript 模拟鼠标键盘操作，实现 macOS 系统的自动化操作 - 少数派](https://sspai.com/post/43758)


### 选中指定 title 的 Chrome tab
```AppleScript
set tabTitle to "jirengu video hotkeys"

tell application "Google Chrome"
	set win_List to every window
	set win_MatchList to {}
	set tab_MatchList to {}
	set tab_NameMatchList to {}
	repeat with win in win_List
		set tab_list to every tab of win
		repeat with t in tab_list
			if tabTitle is in (title of t as string) then
				set end of win_MatchList to win
				set end of tab_MatchList to t
				set end of tab_NameMatchList to (id of win as string) & ".  " & (title of t as string)
			end if
		end repeat
	end repeat
	if (count of tab_MatchList) is equal to 1 then
		set w to item 1 of win_MatchList
		set index of w to 1
		my setActiveTabIndex(t, tabTitle)
	else if (count of tab_MatchList) is equal to 0 then
		display dialog "No match was found!" buttons {"OK"} default button 1
	else
		set which_Tab to choose from list of tab_NameMatchList with prompt "The following Tabs matched, please select one:"
		if which_Tab is not equal to false then
			set oldDelims to (get AppleScript's text item delimiters)
			set AppleScript's text item delimiters to "."
			set tmp to text items of (which_Tab as string)
			set w to (item 1 of tmp) as integer
			set AppleScript's text item delimiters to oldDelims
			set index of window id w to 1
			my setActiveTabIndex(t, tabTitle)
		end if
	end if
end tell

on setActiveTabIndex(t, tabTitle)
	tell application "Google Chrome"
		set i to 0
		repeat with t in tabs of front window
			set i to i + 1
			if title of t contains tabTitle then
				set active tab index of front window to i
				return
			end if
		end repeat
	end tell
end setActiveTabIndex
```

### Chrome 当前 tab 执行 JS
```AppleScript
set jsCode to "alert(1)"
tell application "Google Chrome" to activate

tell application "System Events"
  tell application "Google Chrome"
    tell front window
      tell active tab
        execute javascript jsCode
      end tell
    end tell
  end tell
end tell
```

参考资料：
- [Mac AppleScript实现Chrome浏览器自动化](https://blog.csdn.net/Mr17Liu/article/details/116488957)
- [find a tab by its name in Google Chrome](https://apple.stackexchange.com/questions/273970/applescript-to-find-a-tab-by-its-name-in-google-chrome)