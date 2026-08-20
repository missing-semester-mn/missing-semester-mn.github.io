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

Жил бүрийн лекцүүд бие даасан, бүрэн агуулгатай. Шинээр эхэлж байгаа бол хамгийн сүүлийн хувилбараас нь эхэлсэн нь дээр. Жил бүрийн сэдэв бага зэрэг өөр байдаг тул өмнөх онуудын тэмдэглэл, бичлэгүүдийг энд хадгалж байна.
