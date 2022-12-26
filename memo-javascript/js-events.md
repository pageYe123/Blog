## keydown，keypress，keyup

- `keydown`：按下键盘键
- `keypress`：紧接着`keydown`事件触发（按下字符键和部分功能键时触发）
- `keyup`：释放键盘键

键盘中的键分为**字符键**（可打印）和**系统功能键**（不可打印）

`keypress`支持的系统功能键：

- **Firefox**：`Esc、Enter、Backspace、Pause Break、Insert、 Delete、Home、End、Page Up、Page Down、F1 ~ F12、The Arrow Keys`

- **Chrome / Oprea / Safari** ：`Enter`

- **IE**：`Esc、Enter`



## 页面生命周期事件：DOMContentLoaded，load，beforeunload，unload

HTML 页面的生命周期包含四个重要事件：

| 事件名称         | 所属对象           | 事件说明                                                     |
| ---------------- | ------------------ | ------------------------------------------------------------ |
| DOMContentLoaded | window 或 document | 浏览器已完全加载 HTML，并构建了 DOM 树，处理程序可以查找 DOM 节点，并初始化接口。但像 `<img>` 和样式表之类的外部资源可能尚未加载完成。 |
| load             | window             | 浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。样式已被应用，图片大小也已知了。 |
| beforeunload     | window             | 用户正在离开，我们可以检查用户是否保存了更改，并询问他是否真的要离开。 |
| unload           | window             | 用户几乎已经离开了，但是我们仍然可以启动一些操作，例如发送统计数据。 |

**注意：**load、beforeunload、unload 事件只有 window 才有，document 没有。
