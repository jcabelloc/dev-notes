# How to add Disqus to Github Pages

* Create a disqus account at: https://disqus.com/profile/signup/
* Link that account to one web site, in this case: jcabelloc.github.io
* Create a partial with the HTML that disqus provides in your  _includes/ folder (e.g. _includes/disqus.html)
* disqus.html (reference)
```html
<div id="disqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_shortname = 'jcabelloc'; // required: replace example with your forum shortname
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

```

* Include this partial in your post layout file (e.g. _layouts/post.html)
    * {% include disqus.html %}

* post.html (reference)
```html
---
layout: default
---

<h2 class="post_title">
  {{ page.title }}
  {% if page.date %}
    <small>{{ page.date | date_to_string }}</small>
  {% endif %}
</h2>

{{ content }}


{% if page.comments %} 
  {% include disqus.html %}
{% endif %}
```

* Add a variable called "comments: true" to the YAML Front Matter in every post your want comments
```md
layout: post
comments: true
# other options
```