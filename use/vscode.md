# vscode config

## extension

1. Better Comments
2. Carbon Product icons
3. Bongo Cat
4. Catppuccin Icons for VSCode
5. Code Spell Checker
6. Code runner
7. CodeSnap
8. codelldb
9. Error Lens
10. ESLint
11. ftl xml
12. git graph
13. git history
14. git lens
15. insert number
16. lorem ipsum
17. prisma
18. rust-analyzer
19. Tailwind Css IntelliSense
20. Highlight Matching Tag
21. JavaScript (ES6) code snippets
22. Live Server
23. Path Intellisense
24. Project Manager
25. Regex Previewer
26. SynthWave '84
27. Toggle Quotes
28. Vibrancy Continued
29. vscode-json
30. Vue - Official
31. vue-helper
32. auto import



## setting json

```json
{
  // ========== Visuals ==========
  "editor.smoothScrolling": true,
  "editor.cursorBlinking": "expand",
  "editor.cursorSmoothCaretAnimation": "on",
  "workbench.list.smoothScrolling": true,
  // 鼠标控制大小
  "editor.mouseWheelZoom": true,
  // 彩虹括号与作用域块线条提示
  "editor.guides.bracketPairs": true,
  "editor.bracketPairColorization.enabled": true,
  // 控制活动代码段是否阻止快速建议
  "editor.suggest.snippetsPreventQuickSuggestions": false,
  // 除了 `Tab` 键以外， `Enter` 键是否同样可以接受建议
  // 这能减少“插入新行”和“接受建议”命令之间的歧义
  "editor.acceptSuggestionOnEnter": "smart",
  // 代码补全列表中，优先选择最近的建议
  "editor.suggestSelection": "recentlyUsedByPrefix",
  // 自动补全括号、引号
  "editor.autoClosingBrackets": "beforeWhitespace",
  "editor.autoClosingDelete": "always",
  "editor.autoClosingOvertype": "always",
  "editor.autoClosingQuotes": "beforeWhitespace",
  // 关闭缩进猜测
  "editor.detectIndentation": false,
  "editor.tabSize": 2,
  // 自动换行和行高
  "editor.wordWrap": "on",
  "editor.lineHeight": 1.5,
  // 格式化自动删分号
  "javascript.format.semicolons": "remove",
  "typescript.format.semicolons": "remove",
  "typescript.locale": "zh-CN",
  // 类型提示
  // JS 获得所有类型推导
  "javascript.inlayHints.enumMemberValues.enabled": true,
  "javascript.inlayHints.functionLikeReturnTypes.enabled": false,
  "javascript.inlayHints.parameterNames.enabled": "none",
  "typescript.inlayHints.enumMemberValues.enabled": true,
  "typescript.updateImportsOnFileMove.enabled": "always",
  "javascript.updateImportsOnFileMove.enabled": "always",
  "javascript.preferences.quoteStyle": "single",
  "typescript.preferences.quoteStyle": "single",
  // TS 导入、重命名、补全自动更新相关引用
  "typescript.preferences.preferTypeOnlyAutoImports": true,
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "javascript.suggest.autoImports": true,
  "typescript.suggest.autoImports": true,
  // 折行缩进策略和关闭右侧代码地图
  "editor.minimap.enabled": false,
  "editor.foldingStrategy": "indentation",
  "vue.updateImportsOnFileMove.enabled": true,
  "editor.fontFamily": "'0xProto Nerd Font Propo', Consolas, 'Courier New', monospace",
  "editor.lineNumbers": "interval",
  "editor.renderWhitespace": "boundary",
  "window.autoDetectColorScheme": false,
  "workbench.editor.tabActionLocation": "left",
  "workbench.iconTheme": "catppuccin-mocha",
  "workbench.productIconTheme": "bongocat",
  "workbench.sideBar.location": "right",
  "workbench.activityBar.location": "top",
  "workbench.startupEditor": "newUntitledFile",
  "workbench.tree.expandMode": "singleClick",
  "workbench.tree.indent": 10,
  "workbench.colorTheme": "SynthWave '84",
  // ========== Editor ========== 
  "debug.onTaskErrors": "debugAnyway",
  "diffEditor.ignoreTrimWhitespace": false,
  "editor.wordSeparators": "`~!@#%^&*()=+[{]}\\|;:'\",.<>/?",
  "editor.find.addExtraSpaceOnTop": false,
  "editor.inlineSuggest.enabled": true,
  // "editor.multiCursorModifier": "ctrlCmd",
  "editor.unicodeHighlight.invisibleCharacters": false,
  "editor.stickyScroll.enabled": true,
  "editor.hover.sticky": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": "never",
    "source.fixAll.eslint": "explicit",
    "source.organizeImports": "never"
  },
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  "files.eol": "\n",
  "files.insertFinalNewline": true,
  "files.simpleDialog.enable": true,
  "git.autofetch": true,
  "git.confirmSync": false,
  "git.enableSmartCommit": true,
  "git.untrackedChanges": "separate",
  "scm.diffDecorationsGutterWidth": 2,
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.cursorStyle": "line",
  "terminal.integrated.fontWeight": "300",
  "terminal.integrated.persistentSessionReviveProcess": "never",
  "terminal.integrated.tabs.enabled": true,
  "workbench.editor.closeOnFileDelete": true,
  "workbench.editor.highlightModifiedTabs": true,
  "workbench.editor.limit.enabled": true,
  "workbench.editor.limit.perEditorGroup": true,
  "workbench.editor.limit.value": 15,
  "search.exclude": {
    "**/*.snap": true,
    "**/*.svg": true,
    "**/.git": true,
    "**/.github": false,
    "**/.nuxt": true,
    "**/.output": true,
    "**/.pnpm": true,
    "**/.vscode": true,
    "**/.yarn": true,
    "**/assets": true,
    "**/bower_components": true,
    "**/dist/**": true,
    "**/logs": true,
    "**/node_modules": true,
    "**/out/**": true,
    "**/package-lock.json": true,
    "**/pnpm-lock.yaml": true,
    "**/public": true,
    "**/temp": true,
    "**/yarn.lock": true,
    "**/CHANGELOG*": true,
    "**/LICENSE*": true,
  },
  // ==========  Global Level Config, needs to put in User Settings ==========
  "window.dialogStyle": "custom",
  // "window.nativeTabs": true, // this is great, macOS only
  // "window.title": "${rootName}", // this make tabs more readable
  "window.titleBarStyle": "custom",
  "extensions.autoUpdate": "onlyEnabledExtensions",
  "[jsonc]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[vue]": {
    "editor.defaultFormatter": "Vue.volar"
  },
  "[javascript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  },
  "cSpell.userWords": [
    "autofetch",
    "catppuccin",
    "Consolas",
    "echarts",
    "nuxt",
    "Propo",
    "Vitesse"
  ],
  "vscode_vibrancy.opacity": 0.6,
  "files.autoSave": "afterDelay",
}
```

