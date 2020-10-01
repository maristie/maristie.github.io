---
title: Add Disqus and Mathjax to website
---

The only thing one needs to do is to add codes to blog htmls.

### Disqus

Before adding any code you need to register at **[Disqus](https://disqus.com/)**. Then configure your Disqus short name as a token specified. Now you're prepared for adding a Disqus comment block.

```HTML
<div id="disqus_thread"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    /*
    var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');

        s.src = 'https://[your-short-name].disqus.com/embed.js';

        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
```

### MathJax

That's easy to add Disqus, right? It's even easier to configure **[MathJax](https://www.mathjax.org/)** for you even needn't sign up for any website. Assume you've coding experience in LaTeX. If not, visit [here](https://www.latex-tutorial.com/) for tutorial.

```HTML
<!-- LaTeX support -->
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>
<!-- LaTeX Support -->
```

Attention that according to MathJax (April 18, 2017),

> The MathJax CDN hosted at cdn.mathjax.org will be shutting down on April 30, 2017.
> Current and future releases available on other CDN providers.

So you should select a third-party CDN for processing your LaTeX codes. For more information, visit [here](https://www.mathjax.org/cdn-shutting-down/).

---

_Update_:

New MathJax snippet with CDN provided by Cloudflare:
```
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML'></script>
```

---

_Another Update_:

MathJax 3 web embedding:
```html
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
```

---
