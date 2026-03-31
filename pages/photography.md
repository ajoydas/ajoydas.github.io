---
layout: default
title: Photography
permalink: /photography/
weight: 7
glightbox: true
---
<br/>

## **Photography** 📸

Beyond code and cloud, I find joy in capturing moments through my lens. Photography helps me slow down, observe the world differently, and appreciate the beauty in everyday scenes.

<br/>

{% for category in site.data.photos.categories %}
### {{ category.title }}
{: .gallery-heading}

<div class="photo-gallery" id="gallery-{{ category.id }}" data-initial-count="8">
{% for photo in category.photos %}
  {% assign encoded_file = photo.file | url_encode %}
  <a href="/assets/img/photography/converted/{{ encoded_file }}" class="glightbox photo-item{% if forloop.index > 8 %} photo-hidden{% endif %}" data-gallery="{{ category.id }}" data-description="{{ photo.caption }}">
    <img src="/assets/img/photography/thumbs/{{ encoded_file }}" alt="{{ photo.caption }}" loading="lazy"/>
    <span class="photo-caption">{{ photo.caption }}</span>
  </a>
{% endfor %}
</div>

{% if category.photos.size > 8 %}
<div style="text-align: center; margin-top: 10px;">
  <button class="btn-show-more" data-gallery="gallery-{{ category.id }}" onclick="toggleGallery(this)">
    Show All ({{ category.photos.size }} photos)
  </button>
</div>
{% endif %}

<br/>

{% endfor %}

<hr style="margin-top: 2rem; margin-bottom: 1rem; border-color: #444;">
<p style="text-align: center; font-size: 0.75rem; color: #888;">
  © {{ site.time | date: "%Y" }} Ajoy Das. All rights reserved.<br/>
  All photographs on this page are original works by Ajoy Das. Any use — personal or commercial — requires prior written authorization. Please contact me for licensing inquiries.
</p>
