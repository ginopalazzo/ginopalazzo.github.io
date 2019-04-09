---
layout: post
title:  "Jekyll color code categories"
lang: en
ref: jekyll-color-code-categories
description: "How to implement reusable color code categories en Jekyll."
date:   2017-12-02 11:07:21 +0100
categories: ["Web"]
tags: ["Jekyll", "Visualization"]
---
## Overview

Like in this small blog, usually you will want to organize together posts with similar contents in categories or tags.

What I wanted to do is assign a color to each category in a Jekyll project.

For that, I've created in the root directory a `_categories` directory where each category is going to be stored as a HTML (markdown) file.

{% highlight console %}
.
├── _categories
│   ├── d3js.html
│   ├── data.html
│   ├── jekyll.html
│   ├── python.html
│   └── visualization.html
{% endhighlight %}

The code of a category `_categories/jekyll.html` could be the following:

{% highlight ruby %}
---
title: Jekyll
date: 2016-12-17 11:09:00 -05:00
color: "#ffc107"
---
{% endhighlight %}

In this post `_post/2017-11-20-jekyll-color-code-categories` we'll add a list of categories:

{% highlight ruby%}
---
layout: post
title:  "Jekyll color code categories"
description: "How to implement reusable color code categories en Jekyll."
date:   2017-12-02 11:07:21 +0100
categories: ["Jekyll", "Visualization"]
---
## Overview
Like in this small blog, usually you will want ...
{% endhighlight %}

We also need to add a 'categories' collection in the config file `./_config.yml` :

{% highlight ruby %}
collections:
  - categories
{% endhighlight %}

For displaying the category in a post or in the home page, the snippet that I came up with is the following (probably a better solution could be thought up):

{% highlight liquid %}
    {{ "{% for category in page.categories " }}%}
        {{ "{% for category_template in site.categories " }}%}
            {{ "{% if category == category_template.title " }}%}
            <strong style="color:{{ "{{ category_template.color " }}}};">
                {{ "{{ category " }}}}
            </strong>
            {{ "{% endif " }}%}
        {{ "{% endfor " }}%}  
    {{ "{% endfor " }}%}
{% endhighlight %}

>Was hard to escape liquid template tags like `{{ "{% xxx " }}%)` while writing this post; I found a clean solution for this problem without using any plug-ins in:
[https://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags](https://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags)

The output of this solution is the one you can visualize in the blog.

