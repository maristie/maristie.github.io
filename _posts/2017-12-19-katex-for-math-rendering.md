---
title: KaTeX for math rendering online
---

In [this article](/blog/disqus-mathjax/), I've introduced MathJax as a tool for Web math rendering. If you've been using MathJax, you would probably agree that MathJax is quite slow in formula transformation. Let's compare MathJax with another online math rendering tool, which I'm going to introduce.

![compare](https://raw.githubusercontent.com/Khan/KaTeX/gh-pages/katex-comparison.gif)
_From https://xuc.me/blog/katex-and-jekyll/_

The left one is what I'd like to introduce today - [$$\KaTeX$$](https://khan.github.io/KaTeX/), a substitute for MathJax.

## Advantages

As what I've displayed above, $$\KaTeX$$ is overwhelmingly faster than MathJax (about 50 times!). Here's [a website](https://www.intmath.com/cg5/katex-mathjax-comparison.php) that you could check their speed on your own.

*NOTE*: MathJax 3 has been released, and its performance of math rendering was improved a lot. For this reason, there's no significant drawback in MathJax anymore. Take it easy to deploy either of them.

## Disadvantages

$$\KaTeX$$ is relatively younger than MathJax, and thus is not widely supported by other software like kramdown. And the supported functionality is still a subset of MathJax. However, along with the development [the repo](https://github.com/Khan/KaTeX) has attracted more and more GitHubbers, which seems very promising.

## How to deploy $$\KaTeX$$

**Update**: it's recommended that CSS be placed in `<head>` and JavaScript be placed before `</body>`.

Similar to MathJax we only need to insert several scripts in `<head>` block. You could refer to the official website for guidance (perhaps you'll need [Auto-render](https://github.com/Khan/KaTeX/tree/master/contrib/auto-render), an extension for $$\KaTeX$$). Here I'll show a special case when we're using Jekyll and on GitHub Pages.

First, insert the following code into `<head>`:
```html
<!-- KaTeX CSS -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.9.0-alpha2/katex.min.css" integrity="sha384-exe4Ak6B0EoJI0ogGxjJ8rn+RN3ftPnEQrGwX59KTCl5ybGzvHGKjhPKk/KC3abb" crossorigin="anonymous">
```

Here `jQuery` works as an dependency for the following script. Get the latest jQuery from https://code.jquery.com/.

Since GitHub Pages only supports kramdown and MathJax math engine,[^1][^2] the only method is to parse markdown files with kramdown and use $$\KaTeX$$ to render math formulas.

Here kramdown has provided a solution:[^3]

> To use KaTeX instead of MathJax, activate the MathJax engine in kramdown, include the KaTeX Javascript files in the HTML file and put the following script at the end of the HTML file (note that JQuery is used for some parts but it can probably be done without it):

```html
<!-- jQuery -->
<script src="https://code.jquery.com/jquery-3.2.1.min.js" integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4=" crossorigin="anonymous"></script>

<!-- KaTeX script -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.9.0-alpha2/katex.min.js" integrity="sha384-OMvkZ24ANLwviZR2lVq8ujbE/bUO8IR1FdBrKLQBI14Gq5Xp/lksIccGkmKL8m+h" crossorigin="anonymous"></script>

<!-- Replace MathJax syntax with KaTeX -->
<script>
  $("script[type='math/tex']").replaceWith(function() {
      var tex = $(this).text();
      return katex.renderToString(tex, {displayMode: false});
  });

  $("script[type='math/tex; mode=display']").replaceWith(function() {
      var tex = $(this).html();
      return katex.renderToString(tex.replace(/%.*/g, ''), {displayMode: true});
  });
</script>
```

Be careful that it should be placed at the end of HTML file. For further information, look into [this issue](https://github.com/gettalong/kramdown/issues/292#issuecomment-257770946).

## Demo

In kramdown, both inline and displayed math formulas use **double** dollar signs. They are distinguished from each other by their positions in markdown files.

Here is an example $$ L = \sum_{k=1}^\infty \frac1k $$ for inline math mode, and

$$ \tanh{x} = \dfrac{e^x-e^{-x}}{e^x+e^{-x}} $$

for display style.

```latex
% inline
$$ L = \sum_{k=1}^\infty \frac1k $$

% display
$$ \tanh{x} = \dfrac{e^x-e^{-x}}{e^x+e^{-x}} $$
```

## References

[^1]: https://help.github.com/articles/updating-your-markdown-processor-to-kramdown/

[^2]: https://help.github.com/articles/configuring-jekyll/

[^3]: https://kramdown.gettalong.org/math_engine/mathjax.html
