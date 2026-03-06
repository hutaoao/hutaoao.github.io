---
title: VsCode配色方案-webstorm色调
date: 2025-04-12
description: VsCode配色方案（webstorm色调）
tags: [瑶瑶直上, web]
categories: [瑶瑶直上]
---
# VsCode配色方案（webstorm色调）

一直使用`webstorm`的小伙伴，有个烦恼就是 `webstorm` 收费，每次找个激活码不知道突然哪一天又不能使用了，`vscode` 免费，又习惯webstorm的配色的小伙伴可以参考下：

```json
{
  "workbench.startupEditor": "none",
  "editor.fontSize": 15, // 字体大小
  "editor.tabSize": 2,
  "editor.fontLigatures": false,
  "prettier.bracketSpacing": false,
  "prettier.singleQuote": true,
  "workbench.colorTheme": "Default Dark+",
  "workbench.iconTheme": "material-icon-theme",
  "git.suggestSmartCommit": false,
  "editor.lineHeight": 1.8, // 编辑器行高
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "workbench.colorCustomizations": {
    "editor.background": "#2B2B2B", // 编辑区背景色
    "sideBar.background":  "#3B3F41", // 资源管理器栏背景色
    "statusBar.background":  "#3B3F41", // 底部状态栏背景色
    "activityBar.background": "#3B3F41", // 最左侧活动栏背景色
    "activityBar.border": "#2B2B2B", // 最左侧活动栏边框色
    "editorBracketHighlight.foreground1": "#A9B7C6", // 括号颜色
    "editorBracketHighlight.foreground2": "#A9B7C6",
    "editorBracketHighlight.foreground3": "#A9B7C6",
    "editorBracketHighlight.foreground4": "#A9B7C6",
    "editorBracketHighlight.foreground5": "#A9B7C6",
    "editorBracketHighlight.foreground6": "#A9B7C6",
    "editorBracketHighlight.foreground7": "#A9B7C6",
    "editorBracketHighlight.foreground8": "#A9B7C6",
  },
  "editor.tokenColorCustomizations": {
    "strings": "#6A8759", // 字符串
    "keywords": "#CC7832", // 关键字
    "numbers": "#6897BB", // 数字
    "functions": "#FFC66D", // 函数名
    // "comments": "#808080", // 所有注释
    "types": "#A9B7C6", // 代表着类型的颜
    "variables": "#A9B7C6", // 变量名
    "textMateRules": [
      {
        "scope": "comment.line", // 单行注释
        "settings": {
          "foreground": "#808080"
        }
      },
      {
        "scope": "entity.name.tag", // 标签
        "settings": {
          "foreground": "#f0cb8b",
        }
      },
      {
        "scope": "entity.other.attribute-name", // 标签属性名
        "settings": {
          "foreground": "#A9B7C6"
        }
      },
      {
        "scope": "support.class.component",
        "settings": {
          "foreground": "#FFC66D"
        }
      },
      {
        "scope": "support.type.primitive",
        "settings": {
          "foreground": "#CC7832"
        }
      },
      {
        "scope": "support.variable",
        "settings": {
          "foreground": "#9876AA"
        }
      },
      {
        "scope": "support.type.property-name",
        "settings": {
          "foreground": "#A9B7C6"
        }
      },
      {
        "scope": "storage.modifier",
        "settings": {
          "foreground": "#CC7832"
        }
      },
      {
        "scope": "punctuation.separator.comma",
        "settings": {
          "foreground": "#CC7832"
        }
      },
      {
        "scope": "punctuation.decorator",
        "settings": {
          "foreground": "#BBB529"
        }
      },
      {
        "scope": "punctuation.definition.tag",
        "settings": {
          "foreground": "#FFC66D"
        }
      },
      {
        "scope": "punctuation.separator.key-value",
        "settings": {
        "foreground": "#A9B7C6"
      }
      },
        {
        "scope": "constant.language",
        "settings": {
        "foreground": "#CC7832"
      }
      },
        {
        "scope": "variable.language",
        "settings": {
        "foreground": "#CC7832"
      }
      },
        {
        "scope": "variable.other.property",
        "settings": {
        "foreground": "#9876AA"
      }
      },
        {
        "scope": "variable.other.constant",
        "settings": {
        "foreground": "#A9B7C6"
      }
      },
        {
        "scope": "variable.other.readwrite",
        "settings": {
        "foreground": "#A9B7C6"
      }
      },
        {
        "scope": "variable.other.object.property",
        "settings": {
        "foreground": "#9876AA"
      }
      }
        ],
      },
      }

```


