---
layout: post
title: "Adding support for HTML5's details element to Jekyll"
lang: en
keywords: "HTML5, details, summary, Jekyll, Liquid, tag, tag block"
---

I recently stumbled upon HTML5's `details` element and wanted to use it in
[Jekyll](https://jekyllrb.com/). Here's how I did it.

# Intro

I mostly write in Markdown, so I could just write plain HTML:

{% highlight html %}
<details>
  <summary>Read more about that **thing**...</summary>
  That **thing** is...
</details>
{% endhighlight %}

However, there are two reasons not to use `details` like this:

1. The `details` element is not supported by every browser, so we need some
  kind of a fallback.
2. The text in `details` and `summary` is not processed by Jekyll's markup
  converter, so the text would be rendered as is, i.e.
  "Read more about that \*\*thing\*\*", which is not what we want.

# Polyfill

To address the first point, we can use a *polyfill*. I chose
[jquery-details](https://github.com/mathiasbynens/jquery-details), which, as
the name suggests, depends on [jQuery](https://jquery.com/). Since I didn't
use jQuery until then, I decided to give [Zepto](http://zeptojs.com/) a try,
which is basically a lightweight jQuery.

I included *Zepto* and *jquery-details* in the footer of my page like this:

{% highlight html %}
<script src="{{ "/js/zepto.min.js" | prepend: site.baseurl }}"></script>
<script>jQuery = Zepto;</script>
<script src="{{ "/js/jquery.details.min.js" | prepend: site.baseurl }}"></script>
<script>$('details').details();</script>
{% endhighlight %}

The above lets *jquery-details* run over every `details` element it finds
and polyfills support for it, if necessary.

# Markup Processing

The second point can be addressed by adding a custom plugin. Jekyll uses
[Liquid](https://shopify.github.io/liquid/) as templating engine. In Liquid
you can define custom *tags* and even
[tag blocks](https://github.com/Shopify/liquid/wiki/Liquid-for-Programmers#create-your-own-tag-blocks).
We can create a custom *tag block* to mimic the semantics of the `details`
element and use it like this:

{% highlight liquid %}
{% raw %}
{% details Read more about that **thing**... %}
  That **thing** is...
{% enddetails %}
{% endraw %}
{% endhighlight %}

To create the new tag block, you just put the following tag block
implementation in a *.rb*-file in Jekyll's plugin folder:

{% gist a67fda47e075d2b7fa4764bb42605063 %}

Jekyll will automatically recognize the plugin and it is ready to use.


# Styling

I added some custom SCSS to make the result appear good in different
browsers. The basic functionality works without, but the styling doesn't
really suggest what's going on.

{% highlight SCSS %}
details {
    summary {
        &::before {
            font-family: FontAwesome;
            font-size: 1.2em;
            content: "\f152";
            float: left;
            margin-right: 0.4em;
        }
        &::-webkit-details-marker {
            display: none;
        }
        margin-bottom: 0.8em;
    }
    &.open, &[open] {
        summary::before {
            content: "\f150";
        }
    }
    color: grey;
}
{% endhighlight %}

# Result

Coming full circle, now when we write this:

{% highlight liquid %}
{% raw %}
{% details Read more about that **thing**... %}
  That **thing** is...
{% enddetails %}
{% endraw %}
{% endhighlight %}

This is what Jekyll (and your browser) renders:

{% details Read more about that **thing**... %}
  That **thing** is...
{% enddetails %}

