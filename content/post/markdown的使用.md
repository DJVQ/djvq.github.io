---
title: "markdown的简单使用"
date: 2021-05-13
hidden: false
draft: false
tags: ["学习","markdown"]
keywords: []
description: ""
slug: ""
---

# Markdown 快速入门

## 1 代码块
``````markdown
```javascript
// Fenced **with** highlighting
function doIt() {
    for (var i = 1; i <= slen ; i^^) {
        setTimeout("document.z.textdisplay.value = newMake()", i*300);
        setTimeout("window.status = newMake()", i*300);
    }
}
```
``````
### 示范：
```javascript
function doIt() {
    for (var i = 1; i <= slen ; i^^) {
        setTimeout("document.z.textdisplay.value = newMake()", i*300);
        setTimeout("window.status = newMake()", i*300);
    }
}
```

## 2 标题
```markdown
一个#号为一级标题,两个为两级,以此类推,最多六级
```
### 示范：
# 一
## 二
### 三
#### 四
##### 五
###### 六

## 3 字体
```markdown
//加粗
**内容**
//删除线
~~内容~~
//斜体
*内容*
```

### 示范：

**加粗**
~~删除线~~
*斜体*

## 4 分割线
```
分割线1：
---
分割线2：
***
```
### 示范：

---

***

两种感觉差不多

## 5 图片插入
```
//在线图片：
![在线图片](https://i.loli.net/2019/04/13/5cb1d33cf0ee6.jpg)
//本地图片：
![本地](/100.jpg)
```
### 示范：
//在线图片：
![在线图片](https://i.loli.net/2019/04/13/5cb1d33cf0ee6.jpg)

---

//本地图片：
![本地](/100.jpg)

## 6 超链接

```
//超链接语法
[我的查链接](http:baidu.com)
```

## 示范：

//超链接语法
[我的超链接](http://baidu.com)


## 7 引用
```
> This is a block quote

```

### 示范：

> This is a block quote

## 表格

```
| Colors        | Fruits          | Vegetable         |
| ------------- |:---------------:| -----------------:|
| Red           | *Apple*         | [Pepper](#Tables) |
| ~~Orange~~    | Oranges         | **Carrot**        |
| Green         | ~~***Pears***~~ | Spinach           |
```

### 示范：
| Colors        | Fruits          | Vegetable         |
| ------------- |:---------------:| -----------------:|
| Red           | *Apple*         | [baidu](#Tables) |
| ~~Orange~~    | Oranges         | **Carrot**        |
| Green         | ~~***Pears***~~ | Spinach           |


## 列表
```
//有序
1. First item
2. Second item
3. Third item
//无序
- First item
- Second item
- Third item
```

### 示范：
//有序
1. First item
2. Second item
3. Third item


//无序
- First item
- Second item
- Third item