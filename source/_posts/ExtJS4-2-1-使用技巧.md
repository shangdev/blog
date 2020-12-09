---
title: ExtJS4.2.1 使用技巧
date: 2020-08-06 10:44:55
tags:
- 前端
categories:
- 前端相关
---

ExtJS4.2.1使用技巧。

<!-- more -->

### 1. 通过ID获取DOM，其中 mb2 为ID名称：

```
Ext.get('mb2').on('click', function(e){
    ...
});
```

### 2. Grid actioncolumn handler 列参数：

```
{
    xtype: "actioncolumn",
    items: [{
        icon: "Public/Images/icons/approval.png",
        handler: function(view, rowIndex, colIndex, item, e, record){},
        scope: me
    }],
}
```

### 3. Grid 右键菜单

```
var contextMenu = Ext.create('Ext.menu.Menu', {
    width: 100,
    items: [{
        text: 'Menu_1',
        iconCls: "PSI-button-insertBefore",
        handler: function() {
          alert(1);
        }
    }]
});

var grid = Ext.create("Ext.tree.Panel", {
    listeners: {
        itemcontextmenu: function (grid, record, item, index, e) {
            e.stopEvent();
            contextMenu.showAt(e.getXY());
        }
    }
});
```

### 4. 多附件上传

```
{
    xtype: 'filefield',
    listeners: {
        afterrender:function(fb){
            fb.fileInputEl.set({
                multiple: 'multiple'
            });
        },
        change: function (fb, v) {
            var formData = new FormData();
            var files = fb.fileInputEl.dom.files;
            for (var i = 0; i < files.length; i++) {
                formData.append('file[]', files[i]);
            }
            ...
        }
    },
}
```

### 5. Numberfield 监听事件

```
{
    xtype: 'numberfield',
    minValue: 0,
    listeners: {
        change:function(field, newVal, oldVal){
            ...
        },
    },
}
```

### 6. Form 字段 placeholder属性

```
{
    xtype: 'textfield',
    emptyText: '这里输入placeholder属性值'
}
```

### 7. Grid列锁定后鼠标上下滚动困难

```
# 添加属性至Ext.grid.Panel中
onLockedViewScroll: Ext.emptyFn,
```