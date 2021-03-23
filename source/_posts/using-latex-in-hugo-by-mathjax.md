---
title: 在hugo中通过使用mathjax来显示公式
date: 2019-10-06 21:34:54
categories:
- 博客
tags:
- latex
- hugo
- 博客
---

## 在hugo中通过使用mathjax来显示公式

hugo显示数学公式有两种方式：katex和mathjax。两种方式都很难成功。

先说结果，只有mathjax成功了，方法见[Hugo 静态博客中迄今为止最佳数学公式支持-利用 MathJax & MMark](https://butui.me/post/yet-best-math-formula-support-for-hugo-with-mathjax/)，如果你使用的时候发现并没有完全生效（主要就是`_`、`&`和`\\`会被html渲染）。请查看你采用的主题中是否在其他地方加载了mathjax或者katex，找到它并删掉它就行。

解决方案：假设你的主题名为`hyde`，那么你要在`themes/hyde/layouts/partials`下的`head.html`或者`header.html`或者`footer.html`（我觉得用`head.html`比较好）中添加一行代码：`<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.6/MathJax.js?config=TeX-MML-AM_SVG"></script>`。添加位置为倒数第二行，即在`</head>`或者`</header>`或者`</footer>`的前面一行就行。

当然你也可以用其他的cdn，我觉得cloudflare的速度还算可以，另外MathJax3好像更改了很多东西，不确定是否可用。

另外由于我的主页会集成其他文章的内容，我遇到了一个问题，我的某篇文章中为我用latex生成的一个线路图提供了latex代码，本来这个代码是使用了markdown的代码标记的，但是被集成了之后，通过查看网页源代码发现，由于主页只显示内容，没有代码标记，因此该代码也被mathjax识别了，但是并没有展示成latex公式，造成的显示效果很差，因此我又加入了以下代码：

```html
	<script type="text/x-mathjax-config">
		MathJax.Hub.Config({
			tex2jax: {
				processEscapes: true,
				processEnvironments: false,
				skipTags: ['script', 'noscript', 'style', 'textarea', 'pre']
			}
		});
	</script>
```

主要是`processEnvironments`使得我文本区的元素不会被检查为mathjax公式，具体原因参考[mathjax文档](https://docs.mathjax.org/en/v2.7-latest/options/preprocessors/tex2jax.html)。

关于katex，我找到了以下资料：

- [[hugo]KaTex Intergration](https://dawnarc.com/2019/09/hugokatex-intergration/)
- [Hugo で KaTeX | Atusy’s blog](https://blog.atusy.net/2019/05/09/katex-in-hugo/)
- [Blog养成记(6) Hugo中的LaTeX渲染 | ZHENG Zi’ou](https://orianna-zzo.github.io/sci-tech/2018-03/blog养成记6-hugo中的latex渲染/)
- [Hugo meets kramdown + KaTeX #gohugo | takuti.me](https://takuti.me/note/hugo-kramdown-and-katex/)
- [Hugo+KaTex：在Markdown中嵌入数学公式 · 码文阁(Marvin’s Blog)](https://zh4ui.net/post/2018-06-30-hugo-katex-support/)

关于mathjax，我找到了以下资料：

- [Supported Content Formats | hugo](https://gohugo.io/content-management/formats/#issues-with-markdown)
- [MathJax in Markdown - Doswa](http://doswa.com/2011/07/20/mathjax-in-markdown.html)
- [Setting MathJax with Hugo | Hi, I am David](https://divadnojnarg.github.io/blog/mathjax/)
- [[solved] MathJax stopped working - support - Hugo](https://discourse.gohugo.io/t/solved-mathjax-stopped-working/5946)
- [MathJax 与 Markdown 的究极融合 - Yihui Xie | 谢益辉](https://yihui.name/cn/2017/04/mathjax-markdown/)
- [The Best Way to Support LaTeX Math in Markdown with MathJax - Protect math as code, and remove those code tags via JavaScript later  - Yihui Xie | 谢益辉](https://yihui.name/en/2018/07/latex-math-markdown/)
- [使用Pandoc和KaTeX为HUGO添加LaTeX支持 - RayHY](https://yihui.name/en/2018/07/latex-math-markdown/)
- [在Hugo中使用MathJax · 零壹軒·笔记](https://note.qidong.name/2018/03/hugo-mathjax/)
- [Hugo 集成 MathJax](https://blog.pytool.com/language/golang/hugo/hugo_mathjax/)
- [[hugo]集成MathJax]([https://dawnarc.com/2018/06/hugo%E9%9B%86%E6%88%90mathjax/](https://dawnarc.com/2018/06/hugo集成mathjax/))
- [Hugo 静态博客中迄今为止最佳数学公式支持-利用 MathJax & MMark](https://butui.me/post/yet-best-math-formula-support-for-hugo-with-mathjax/)

最后，我衷心地希望我们可以自由地在各个平台上使用markdown，并且用`$inline style$`来表示行内公式，用`$$display style$$`来表示块级公式，用`\$`来表示\$符号，而不用担心能否完美支持。我相信，这一天一定会到来的。