---
title: "Customisation"
date: 2025-10-05
tags: ["blog","custom"]
---

## hover 

Hover the mouse over the {{< term "world" "more introductions" >}} to see more introductions.

```text
{{</* term "world" "more introductions" */>}}
```

{{< details summary="**custom.css**" >}}
```css
/* assets/css/custom.css */
/* 专业词汇容器 */
.custom-term {
  position: relative;
  border-bottom: 1px dashed #4299e1; /* 蓝色虚线提示可悬浮 */
  cursor: help;
  margin: 0 2px;
}

/* 悬浮提示框（核心：增大font-size） */
.custom-term::after {
  content: attr(data-tooltip);
  position: absolute;
  left: 50%;
  bottom: 100%;
  transform: translateX(-50%); /* 水平居中 */
  margin-bottom: 8px;
  padding: 10px 14px;
  background: #2d3748; /* 深色背景 */
  color: white;
  font-size: 16px; /* 字体放大（默认14px→16px） */
  line-height: 1.6; /* 行高同步调整，增强可读性 */
  max-width: 300px; /* 限制最大宽度，避免过宽 */
  border-radius: 6px;
  box-shadow: 0 3px 10px rgba(0,0,0,0.2);
  z-index: 9999;
  display: none;
  white-space: normal; /* 允许文本换行 */
}

/* 鼠标悬浮时显示提示框 */
.custom-term:hover::after {
  display: block;
}

/* 提示框箭头（可选） */
.custom-term::before {
  content: "";
  position: absolute;
  left: 50%;
  bottom: calc(100% - 5px);
  transform: translateX(-50%);
  border-width: 5px;
  border-style: solid;
  border-color: #2d3748 transparent transparent transparent;
  display: none;
}

.custom-term:hover::before {
  display: block;
}
```
{{< /details >}}

{{< details summary="**term.html**" >}}
```html
<!-- layouts/shotcodes/term.html -->
<span class="custom-term" data-tooltip="{{ .Get 1 | safeHTML }}">
  {{ .Get 0 }}
</span>
```
{{< /details >}}