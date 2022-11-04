

## 一、color-scheme

作用：应用当前系统配色方案到元素上

语法：

```css
color-scheme: normal;
color-scheme: light;
color-scheme: dark;
```

示例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Document</title>
</head>

<body>
    <textarea id="example-element-1" style="color-scheme: normal;" rows="16">Text Area normal</textarea>
    <textarea id="example-element-2" style="color-scheme: dark;" rows="16">Text Area dark</textarea>
    <textarea id="example-element-3" style="color-scheme: light;" rows="16">Text Area light</textarea>
    <button style="color-scheme: normal;">click me</button>
    <button style="color-scheme: dark;">click me</button>
    <button style="color-scheme: light;">click me</button>
</body>

</html>
```



## 二、JS 检测系统当前配色模式

```js
// 检测系统当前是否为深色模式，返回布尔值
window.matchMedia("(prefers-color-scheme: dark)").matches
// Mac 通过「系统偏好设置」→ 通用，可以进行外观配色模式变更
```

## 三、@media 适配系统深浅色模式

语法：

```css
/* 深⾊模式 当系统处于黑色模式时，加载如下样式 */
@media (prefers-color-scheme: dark) {}
/* 浅⾊模式 同上*/
@media (prefers-color-scheme: light) {}
```

示例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Document</title>
</head>

<body>
    <style>
        .day {
            background: #eee;
            color: black;
        }

        .night {
            background: #333;
            color: white;
        }

        @media (prefers-color-scheme: dark) {
            .day.dark-scheme {
                background: #333;
                color: white;
            }

            .night.dark-scheme {
                background: black;
                color: #ddd;
            }
        }

        @media (prefers-color-scheme: light) {
            .day.light-scheme {
                background: white;
                color: #555;
            }

            .night.light-scheme {
                background: #eee;
                color: black;
            }
        }

        .day,
        .night {
            display: inline-block;
            padding: 1em;
            width: 8em;
            height: 2em;
            vertical-align: middle;
        }
    </style>
    <div class="day">Day (initial)</div>
    <div class="day light-scheme">Day (changes in light scheme)</div>
    <div class="day dark-scheme">Day (changes in dark scheme)</div>
    <br />

    <div class="night">Night (initial)</div>
    <div class="night light-scheme">Night (changes in light scheme)</div>
    <div class="night dark-scheme">Night (changes in dark scheme)</div>
</body>

</html>
```

## 四、参考资料

https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme

https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme