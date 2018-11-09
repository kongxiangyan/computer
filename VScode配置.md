## 设置

https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync

## 拓展

### Settings Sync

> 迁移和同步VSCode配置

GitHub Token: 7336d78e7ae16754647b4d185830be809f358618

GitHub Gist: 132b5f684408cff41e2a5907cd0197e8

GitHub Gist Type: Secret

### Auto Close Tag

### Auto Rename Tag

### Document This

`Ctrl+Alt+D` and again `Ctrl+Alt+D`

```json
{
    "docthis.includeDateTag": true,
    "docthis.includeDescriptionTag": true,
    "docthis.includeAuthorTag": true,
    "docthis.authorName": "cigaret"
}
```

### npm Intellisense

### Path Intellisense

### IntelliSense for CSS class names in HTML

If there are HTML or JS files on your workspace, the extension automatically starts and looks for CSS class definitions. In case new CSS classes are defined, or new CSS files are added to the workspace, and you also want auto-completion for them, just hit the lightning icon on the status bar. Also, you can execute the command by pressing `Ctrl+Shift+P`(`Cmd+Shift+P` for Mac) and then typing "Cache CSS class definitions."

### JavaScript (ES6) code snippets

### Bracket Pair Colorizer

### ESLint 

### filesize

### Regex Previewer

### TODO Highlight

### Easy SASS

```json
{
    "easysass.formats": [
        {
            "format": "expanded",
            "extension": ".css"
        },
        {
            "format": "compressed",
            "extension": ".min.css"
        }
    ],
    "easysass.targetDir": "",
}
```

### Angular 7 Snippets

### gitignore

## 支持Python

### Jupyter

- use `#%%` to add a run cell button.

- If you want to run cell with Ctrl+Enter, add those code in keybindings.json.

```json
{
    "key": "ctrl+enter",
    "command": "jupyter.execCurrentCell",
    "when": "editorTextFocus"
}
```

### Visual Studio Code Tools for AI

https://github.com/Microsoft/vscode-tools-for-ai

#### Problem

1. **已经安装了python和jupyter，但是就是报错**
   修改PythonPath变量：打开`File->preferences->settings`, 以.json方式显示，查找python.pythonPath, 修改其为`"python.pythonPath": “python安装路径\pythonw.exe”`.

2. **右键打开没有显示**
   确保目录：`（用户名）\.jupyter\下存在jupyter_notebook_config.json`，并且其内容为：

   ```json
   {
       "NotebookApp": {
           "tornado_settings": {
               "headers": {
                   "Content-Security-Policy": "frame-ancestors  file://* "
               }
           }
       }
   }
   ```

### Python

### Python Preview

### Visual Studio IntelliCode

