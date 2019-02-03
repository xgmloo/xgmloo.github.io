---
layout: post
title:  "vscode .vue 自动闭合HTML标签"
date:   2019-02-03
categories: vscode vue
---

进入`vscode-file-preferences-settings`

随便找一个`Edit in settings.json`点进去，在右侧进行编辑，添加代码：

```
"files.associations": {
    "*.vue": "html"
},
"emmet.triggerExpansionOnTab": true,
"emmet.includeLanguages": {
    "vue-html": "html",
    "vue": "html"
}
```

保存之后就可以了。

但是，

`elementUI`的标签就不会自动补全了...