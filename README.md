---
title: Notebook of Hengxing
---
## Welcome to my notes pages
This project is destinated for doc my experiences.

## Catalog

{% for category in site.categories %}
<h2>{{ category | first }}</h2>
</span>{{ category | last | size }}</span>
<ul class="arc-list">
    {% for post in category.last %}
        <li>{{ post.date | date:"%d/%m/%Y"}}<a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
{% endfor %}

### Recommendation System

1. [Sparse LInear Method](recosys/SLIM)
2. [Global Local Sparse LInear Method](recosys/GLSLIM)

### Machine Learning

1. [Logistic Regression](ml/Logistic Regression)

### Algorithms

1. [Reservoir Sampling Algorithms](algo/Reservoir Sampling Algorithms)

### Programs

1. [Two Sum](prog/2sum)
2. [Rotate List](prog/Rotate List)
3. [Intersection of Two Linked Lists](prog/Intersection of Two Linked Lists)
