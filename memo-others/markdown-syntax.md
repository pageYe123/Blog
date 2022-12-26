## 在反引号对语句中使用反引号

- 如果需要在“特殊文字”包含一对反引号，那外面需要使用`两反引号对+空格`来包围这个“特殊文字”

    ```markdown
    `` 含有`反引号`的特殊文字 ``
    ```

    效果：`` 含有`反引号`的特殊文字 ``

    如果要显示的反引号对中没有文字，需要三个反引号：

    ```markdown
    ``` 请使用反引号对（``） ```
    ```

    效果：```请使用反引号对（``）```

- 如果需要在“特殊文字”包含两个引号对，那外面需要使用`三引号对 + 空格`来包围这个“特殊文字”。

    ```markdown
    ```含有``二反引号对``的特殊文字 ```
    ```

    效果：```含有``二反引号对``的特殊文字```
    
    

## 在 table 中引入 code block

GitHub 中支持该语法，Typora 不支持。

````html
<table>
<tr>
<td> Status </td><td> Response </td>
</tr>
<tr>
<td> 200 </td>
<td>

<!-- ⬆︎ Extra blank line above needed! -->
```json
{
    "id": 10,
    "username": "alanpartridge",
    "email": "alan@alan.com",
    "password_hash": "$2a$10$uhUIUmVWVnrBWx9rrDWhS.CPCWCZsyqqa8./whhfzBZydX7yvahHS",
    "password_salt": "$2a$10$uhUIUmVWVnrBWx9rrDWhS.",
    "created_at": "2015-02-14T20:45:26.433Z",
    "updated_at": "2015-02-14T20:45:26.540Z"
}
```

</td>
</tr>
<tr>
<td> 400 </td>
<td>

**Markdown** _here_. (⬆︎ Extra blank line above needed!)

</td>
</tr>
</table>
````

 font-family: ui-monospace,SFMono-Regular,SF Mono,Menlo,Consolas,Liberation Mono,monospace;

Hiragino Maru Gothic ProN

⬆⬇⬅➡

↑↓←→ a

⬆️⬇️⬅️➡️



