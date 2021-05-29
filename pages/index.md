---
layout: page
title: Cesar Viana
permalink: /
weight: 3
---

# **About Me**

{% include landing.html %}

Hi I am **{{ site.author.name }}** :wave:<br>

Welcome to my online profile. Here I keep tracking of my programming skills, working experiences and education. I also have a [blog](/blog), where I try to keep track of my experiences as a software developer. 

About me, I’ve a Computer Science degree, and working experience with **VueJS**, **PHP**, **Java** and **JavaScript**. My life goal is to build applications to produce good impact in the world. I enjoy working in democratic teams, where developers can share their creativity and they are free to build. I like clean code, and think that to be clean, we must refactor early and often.

My actual bucket list :pray: is:
{% include checklist.html items=site.data.bucket-list %}

Here are some websites and pages I’ve developed or contributed to:

- [Eletroblocks](https://eletroblocks.com.br)
- [Lab publications page](https://lite.acad.univali.br/publicacoes-2)
- [TVGaspar](https://tvgaspar.com.br)

Actually finishing master's in Applied Computing. Here are my work on that.

- [Designing Programmable Toy’s Interfaces for Small Children](https://estudosemdesign.emnuvens.com.br/design/article/view/1150/466)
- [Dissertation repository](https://github.com/cesarviana/dissertation)

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.skills.programming %}
{% include about/skills.html title="Other Skills" source=site.data.skills.other %}
</div>

<div class="row">
{% include about/timeline.html title="Experience" source=site.data.timeline.experience %}
</div>

<div class="row">
{% include about/timeline.html title="Education" source=site.data.timeline.education %}
</div>