---
layout: page
title: My Portfolio
permalink: "/portfolio/"
images:
- url: assets/img/hire.jpg
---
<!-- START PORTFOLIO -->
<section id="portfolio">
    <div class="container">
        <div class="row">
            <div class="col-md-12 col-sm-12">
                <div class="col-md-12 filter-btn text-center">
                    <div class="filter active" data-filter="all">All</div>
                    {% assign tags =  site.portfolio | map: 'tags' | join: ','  | split: ',' | uniq | sort %}
                    {% for tag in tags %}
                      <div class="filter {% if forloop.last == true %}lst-cld{% endif %}" data-filter=".{{ tag }}">{{ tag | capitalize }}</div>
                    {% endfor %}
                </div>
                <div id="port-image" class="container">
                    <div class="row">
                      {% for item in site.portfolio %}
                        <div class="col-md-4 col-sm-6 col-xs-12 grid mix {% for tag in item.tags %}{{ tag }} {% endfor %}">
                            <a href="{{ item.url | relative_url }}" class="">
                                <figure class="port-item">
                                    <img src="{{ item.images[0].url | relative_url }}" class="img-responsive" alt="{{ item.images[0].alt }}">
                                    <figcaption>
                                        <h5>{{ item.title }}</h5>
                                        <p class="description">{{ item.images[0].caption }}</p>
                                    </figcaption>
                                </figure>
                            </a>
                        </div>
                      {% endfor %}
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
<!-- END PORTFOLIO -->