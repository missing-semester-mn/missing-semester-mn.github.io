---
layout: page
title: Өмнөх жилүүдийн хичээлүүд
description: >
  Компьютерийн ухааны боловсролын орхигдсон семестр хичээлийн өмнөх онуудын материалыг эндээс олно уу.
---

{% comment %} pop to remove default "posts" collection {% endcomment %}
{% assign sorted_collections = site.collections | sort: 'label' | pop | reverse %}
<ul>
{% for collection in sorted_collections %}
    <li><a href="/{{ collection.label }}/">{{ collection.label }}</a></li>
{% endfor %}
</ul>

Жил бүрийн лекцүүд нь бие даасан, бүрэн агуулгатай байдаг. Бид танд материалын хамгийн сүүлийн хувилбараас эхлэхийг зөвлөж байна. Жил бүрийн лекцүүдэд үзэх сэдвүүд өөр өөр байдаг тул өмнөх онуудын тэмдэглэл болон видео бичлэгүүдийг үргэлжлүүлэн байршуулсаар байна.
