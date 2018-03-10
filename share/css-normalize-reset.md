# Normalize.css vs Reset.css
Normalize.css 相比于传统的 CSS reset 来说，它是一种现代的、为HTML5准备的优质替代方案。它使浏览器更佳一致的呈现所有元素，并且符合现代标准，它只针对那些需要规范化的样式。它非常非常的轻量级，以至于源代码也只有 6KB 左右。现在已经被用于Twitter Bootstrap、HTML5 Boilerplate、GOV.UK、Rdio、CSS Tricks 以及许许多多其他框架、工具和网站上。

## 概述
Normalize.css 是一个 CSS resets 代替方案。经过 [@necolas](https://twitter.com/necolas) 和 [@jon neal](https://twitter.com/jon_neal) 花了几百个小时来努力研究不同浏览器的默认样式的差异，这个项目终于变成了现在这样。

Normalize.css 的目标：

* **保护有用的浏览器默认样式**而不是去掉它们；
* **正常化样式**来服务于更多的 HTML 元素；
* **修复浏览器的 Bug**并确保浏览器的一致性；
* **优化可用性**通过细微的改进；
* **解释代码**通过注释和详细的文档；

它支持大部分浏览器（包括移动端浏览器），包括规范了 HTML5 元素、排版、列表、嵌入内容、表单和表格的 CSS。

尽管该项目基于规范化原则，但是还是在适合的地方使用了更佳实用的默认值。

## Normalize vs Reset
我们是有必要去了解更多的关于 Normalize 和传统的 CSS resets 之间的区别的：

### Normalize.css 保护有用的默认值
Resets 几乎将所有元素的默认样式重新设置，强行将默认样式设置了同样的视觉效果。相比于 Normalize.css，它保留了浏览器的许多默认样式。这意味着你不必为所有常见的排版元素重新声明样式。

当 HTML 元素在不同的浏览器中有不同的默认样式时，Normalize.css 的目的在于让这些样式更佳的一致并符合现代浏览器的标准。

### Normalize.css 修复了浏览器的 Bug
它解决了桌面和移动端浏览器的常见 bug，当然这些 bug 超出了 Resets 的范围。这些 bug 包括 HTML5 元素的显示设置、预格式化文本的字体大小、IE9 中 SVG 的溢出以及浏览器和操作系统中众多表单相关的 bug。

例如，下面是 Normalize.css 修复了 HTML5 input 的 search 类型的跨浏览器一致性和样式的 bug：

```
/**
 * 1. Addresses appearance set to searchfield in S5, Chrome
 * 2. Addresses box-sizing set to border-box in S5, Chrome (include -moz to future-proof)
 */

input[type="search"] {
  -webkit-appearance: textfield; /* 1 */
  -moz-box-sizing: content-box;
  -webkit-box-sizing: content-box; /* 2 */
  box-sizing: content-box;
}

/**
 * Removes inner padding and search cancel button in S5, Chrome on OS X
 */

input[type="search"]::-webkit-search-decoration,
input[type="search"]::-webkit-search-cancel-button {
  -webkit-appearance: none;
}
```

Resets 通常错误的将所有浏览器都一致的设置元素的呈现方式。特别是对于表单来说，Normalize.css 会提供更佳重要的帮助。

### Normalize.css 不会让你的调试工具杂乱无章
使用 Reset 时，最大的灾难是在浏览器 CSS 调试工具中使用大段的继承链。

在 Normalize.css 中不会存在这样的问题，因为在我们的准则中对多选择器的使用是非常谨慎的，我们只会有目的地对元素设置样式。

### Normalize.css 是模块化的
这个项目已经拆分成多个相对独立的部分，让你更佳容易的区分哪些元素设置了特定的样式。此外，它可以让你去除你不需要的部分（例如，表单模块）。

### Normalize.css 拥有丰富的文档
Normalize.css 的代码是基于跨浏览器的设计和系统性的测试。它在 [Github Wiki](https://github.com/necolas/normalize.css) 上有详细的文档和说明。这意味着你可以知道每一行代码的意义：为什么有这些代码、浏览器之间的不同并且可以简单的运行在自己的测试中。

这个项目旨在帮助人们了解浏览器如何在默认情况下渲染元素，并且使得它们更佳容易参与和改进。

## 如何使用
首先，安装或者从 GitHub 中下载 Normalize.css，接下来有两种途径来使用它：

* 用 Normalize.css 当作你自己的基础样式，自定义样式值来符合设计师的标准；
* 引入 Normalize.css 源码并从基础上开始构建，有必要时需要用到你自己的 CSS 覆盖默认值。

## 结束语
Normalize.css 在适用范围上与 CSS reset 上有很大的不同之处。我们值得尝试一下，看看它是否符合你的开发方式和偏好。