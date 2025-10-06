---
title: "个人常用简码"
date: 2025-10-06
---

## Hugo 简码
There are some Hugo's shortcodes I often use.Learn more on this [website](https://gohugo.io/shortcodes/).
### details
隐藏文本细节
{{< details summary="show details" >}}
details
{{< /details>}}
```text
{{</* details summary="show details" */>}}
show details
{{</* /details */>}}
```
### hightlight
将文本展示为代码快并将一部分高亮
{{< highlight bash " hl_lines=8">}}
What now> d
diff --git a/staged_code.txt b/staged_code.txt
new file mode 100644
index 0000000..e1c2e75
--- /dev/null
+++ b/staged_code.txt
@@ -0,0 +1 @@
+this is a staged code
\ No newline at end of file
{{</highlight>}}

how to use it
```text
{{</* highlight bash " hl_lines=8"*/>}}
some codes here
{{</*/highlight */>}}
```
## blowfish 简码
There are some blowfish's shortcodes I often use.You can learn more on this [website](https://blowfish.page/zh-cn/docs/shortcodes/)

### article
Article 将把一篇文章嵌入到一个 markdown 文件中。 参数中的 link应该是要嵌入的文件的 .RelPermalink。请注意，如果简码引用其父级文件，则它不会显示任何内容。
{{< article link="/blog/custom/" showSummary=true compactSummary=true >}}

how to use it

```text
{{</* article link="/blog/custom/" showSummary=true compactSummary=true */>}}
```
### alert
{{< alert icon="fire" cardColor=" #e63946"iconColor="#866b67ff" textColor="#f1faee" >}}
增强显示
{{< /alert >}}
```text
{{</* alert icon="fire" cardColor=" #e63946"iconColor="#866b67ff" textColor="#f1faee" */>}}
增强显示
{{</* /alert*/>}}
```
## 定制简码

### hover

Hover the mouse over the {{< term "world" "more introductions" >}} to see more introductions.

```text
{{</* term "world" "more introductions" */>}}
```

You want to use it? [See this here](#article)