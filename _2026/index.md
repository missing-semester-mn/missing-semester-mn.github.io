---
layout: page
title: "2026 оны лекцүүд"
description: >
  Компьютерийн ухааны боловсролын орхигдсон семестр, MIT IAP 2026 хичээлийн лекцийн тэмдэглэл болон видео бичлэгүүд.
permalink: /2026/
phony: true
---

<ul class="double-spaced">
  {% assign lectures = site['2026'] | sort: 'date' %}
  {% for lecture in lectures %}
    {% if lecture.phony != true %}
      <li>
        <strong>{{ lecture.date | date: '%-m/%-d' }}</strong>:
        {% if lecture.ready %}
          <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% elsif lecture.noclass %}
          {{ lecture.title }} [хичээл ороогүй]
        {% else %}
          {{ lecture.title }} [удахгүй орно]
        {% endif %}
        {% if lecture.details %}
          <br>
          ({{ lecture.details }})
        {% endif %}
      </li>
    {% endif %}
  {% endfor %}
</ul>

Лекцийн видео бичлэгүүдийг [YouTube хуудаснаас](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L)
үзэх боломжтой.

# MIT-ээс гадуур

Бусад хүмүүст ч мөн эдгээр нөөц боломжууд ач тусаа өгнө гэж найдан бид энэхүү
хичээлийг MIT-ээс гадуур хуваалцсан. Та дараах платформ дээрх нийтлэл,
хэлэлцүүлгүүдийг үзэх боломжтой:

- [Hacker News](https://news.ycombinator.com/item?id=47124171)
- [Lobsters](https://lobste.rs/s/q4ykw7/missing_semester_your_cs_education_2026)
- [r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/1r93yk6/the_missing_semester_of_your_cs_education_2026/)
- [X](https://x.com/anishathalye/status/2024521145777848588)
- [Bluesky](https://bsky.app/profile/jonhoo.eu/post/3mfa2bhyuj22i)
- [Mastodon](https://fosstodon.org/@jonhoo/116098318361854057)
- [LinkedIn](https://www.linkedin.com/posts/anishathalye_i-returned-to-mit-during-iap-january-term-activity-7430285026933522433-Ehr9)
- [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L)

# Талархал

Лекцийн видеог бичих боломжийг олгосон Elaine Mello болон [MIT Open Learning](https://openlearning.mit.edu/)-д
талархал илэрхийлье. Мөн [SIPB IAP 2026](https://sipb.mit.edu/iap/)-ийн
хүрээнд энэхүү хичээлийг дэмжсэн Luis Turino / [SIPB](https://sipb.mit.edu/)-д
талархал илэрхийлье.
