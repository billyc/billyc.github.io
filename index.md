---
layout: page
title: 'Home'
class: home
---

# Hi there, I'm Billy Charlton

<div class="columns" markdown="1">

<div class="intro" markdown="1">
I am a researcher at [Technische Universität Berlin](https://vsp.tu-berlin.de) focusing on **data visualization** for the [MATSim transport simulation framework](https://matsim.org). MATSim is currently getting a lot of attention for its ability to simulate [COVID-19 virus propagation](https://covid-sim.info) as well as traffic scenarios for the impending doomsday arrival of robot cars.

Before coming to Berlin, I was **Director of Data** for the [Puget Sound Regional Council](https://www.psrc.org) in Seattle, USA. Our 22-person team managed all travel forecasting and land use / growth management in support of the Seattle metropolitan region’s planning goals.

In 2023 I completed my Ph.D. dissertation from [Technische Universität Berlin](https://vsp.berlin/), under direction of Professor Kai Nagel at the Institute for Land- and Sea Transport, Transport Systems Planning and Telematics.

I received my Masters in Civil Engineering from the [Northwestern University Transportation Center](https://www.transportation.northwestern.edu/) and my undergraduate BSCE from [Cornell University](https://www.cee.cornell.edu/cee).

I am always looking for new interesting projects! Email me or find me on Twitter at @billyinberlin.

</div>

<div class="me" markdown="1">
  <picture>
    <source srcset='/images/headshot3.jpg' type='image/jpg' />
    <img
      src='/images/headshot3.jpg'
      alt='Billy Charlton'/>
  </picture>

{:.no-list}

- <a href="mailto:{{ site.email }}">{{ site.email }}</a>
</div>

</div>

<!-- During my first year at UW, I received support from the [Fulbright program](https://en.wikipedia.org/wiki/Fulbright_Program). In 2013, I received my B.S. from [Hasso Plattner Institute](https://hpi.de/). I am a scholar of the [German National Academic Foundation](http://www.studienstiftung.de/). I have worked with the [Open Knowledge Foundation](http://www.okfn.org), [Google Research](https://ai.google/research/), and [Microsoft Research](https://www.microsoft.com/en-us/research/group/vibe/). Details are in my [CV]({{ "/cv/" | relative_url }}).
-->

## Featured Projects

<div class="featured-projects">
  {% assign sorted_projects = site.data.projects | sort: 'highlight' %}
  {% for project in sorted_projects %}
    {% if project.highlight %}
      {% include project.html project=project %}
    {% endif %}
  {% endfor %}
</div>
<a href="{{ "/projects/" | relative_url }}" class="button">
  <i class="fas fa-chevron-circle-right"></i>
  Show More Projects
</a>

## Featured Publications

<div class="featured-publications">
  {% assign sorted_pubs = site.publications | sort: 'highlight' %}
  {% for pub in sorted_pubs %}
    {% if pub.highlight %}
      <a href="{{ pub.pdf }}" class="publication">
        <strong>{{ pub.title }}</strong>
        <span class="authors">{% for author in pub.authors %}{{ author }}{% unless forloop.last %}, {% endunless %}{% endfor %}</span>.
        <i>{{ pub.venue }}, {{ pub.year }}</i>.
        {% for award in pub.awards %}<br/><span class="award"><i class="fas fa-{% if award == "Best Paper Award" %}trophy{% else %}award{% endif %}" aria-hidden="true"></i> {{ award }}</span>{% endfor %}
      </a>
    {% endif %}
  {% endfor %}
</div>

<a href="{{ "/publications/" | relative_url }}" class="button">
<i class="fas fa-chevron-circle-right"></i>
Show All Publications
</a>

## Recent Blog Entries

<div class="featured-projects">
  {% assign top_posts = site.posts | sort: 'date' | reverse %}
  {% for blog in top_posts limit:3 %}
      {% include blog-blurb.html blog=blog %}
  {% endfor %}
</div>
<a href="{{ "/blog/" | relative_url }}" class="button">
<i class="fas fa-chevron-circle-right"></i>
Show All Blog Entries
</a>

<div class="news-travel" markdown="1">

<div class="news" markdown="1">
## Latest Personal News

<ul>
{% for news in site.data.news limit:10 %}
  {% include news.html news=news %}
{% endfor %}
</ul>

</div>

<div class="travel" markdown="1">
## Latest Travel & Conferences

<table>
<tbody>
{% assign future_travel = site.data.travel | where_exp:'item','item.start == null' %}
{% for travel in future_travel %}
  {% include travel.html travel=travel %}
{% endfor %}
{% assign sorted_travel = site.data.travel | where_exp:'item','item.start' | sort: 'start' | reverse %}
{% for travel in sorted_travel limit:10 %}
  {% include travel.html travel=travel %}
{% endfor %}
</tbody>
</table>

</div>

</div>
