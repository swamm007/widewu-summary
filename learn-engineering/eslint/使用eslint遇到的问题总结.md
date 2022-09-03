## eslint 配置配好之后不检测ts文件
```js
  // 在eslint配置文件中新增以下配置
  parserOptions: {
    ecmaVersion: 'latest',
    project: ['tsconfig.json']
  },
  // 在vscode setting.json中新增
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue",
    "typescript",
    "typescriptreact"
  ]
```
## vscode默认保存的时候的格式化规则和eslint的不一致
* 需要关闭掉vscode的默认保存时候格式化。同时启用保存时候使用eslint格式化
```js
// "editor.formatOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue",
    "typescript",
    "typescriptreact"
  ],
  "editor.codeActionsOnSave": {
    "source.organizeImports": true,
    "source.fixAll.eslint": true
  },
```

## 一份完整的vacode中setting.json配置如下

```js
{
  "workbench.colorTheme": "Default Dark+",
  "security.workspace.trust.untrustedFiles": "open",
  "markdown-image.local.path": "/md-images",
  "markdown-preview-enhanced.previewTheme": "solarized-light.css",
  "markdown-image.coding.path": "/learn-log/images",
  "window.zoomLevel": 1,
  "editor.tabSize": 2,
  // "editor.formatOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue",
    "typescript",
    "typescriptreact"
  ],
  "editor.codeActionsOnSave": {
    "source.organizeImports": true,
    "source.fixAll.eslint": true
  },
  "editor.fontLigatures": false,
  "eslint.workingDirectories": []
}
```