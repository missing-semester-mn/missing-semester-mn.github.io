---
layout: page
title: "Компьютерийн ухааны боловсролын орхигдсон семестр"
description: >
  Танд илүү бүтээмжтэй компьютерийн ухааны мэргэжилтэн, програмчлагч болоход туслах хүчирхэг хэрэгслүүдийг эзэмшинэ үү.
# subtitle: IAP 2026
subtitle: "2026"
nositetitle: true
---

Их сургуулийн хичээлүүдээр үйлдлийн системээс эхлээд машин сургалт хүртэлх CS-ийн гүнзгийрүүлсэн сэдвүүдийг заадаг боловч оюутнуудад өөрсдөө бие даан сурахыг даатгаж орхидог, бараг заагддаггүй нэгэн чухал сэдэв байдаг. Энэ бол хэрэгслүүдээ чадварлаг ашиглах явдал юм. Бид танд командын мөрийг хэрхэн төгс эзэмших, хүчирхэг текст засварлагч ашиглах, хувилбар хяналтын системийн сонирхолтой функцуудыг хэрхэн ашиглах зэрэг олон зүйлийг заах болно!

Оюутнууд суралцах хугацаандаа эдгээр хэрэгслийг ашиглахад олон зуун цагийг, карьерынхаа явцад олон мянган цагийг зарцуулдаг тул тэдгээрийг ашиглах туршлагыг аль болох саадгүй, хялбар болгох нь зүйтэй юм. Эдгээр хэрэгслийг эзэмшсэнээр тэднийг өөрийн хүссэнээр ажиллуулах гэж цаг алдахгүй байгаад зогсохгүй, урьд нь шийдвэрлэх боломжгүй мэт санагдаж байсан нарийн төвөгтэй асуудлуудыг шийдвэрлэх боломжийг танд олгоно.

Өнөө үед AI-д суурилсан болон AI-аар сайжруулсан хэрэгслүүд, ажлын урсгалууд нэвтэрснээр програм хангамжийн инженерчлэлийн олон тал хурдацтай өөрчлөгдөж байна. Эдгээрийг сул талуудыг нь ухамсарлан, зохистой ашиглавал компьютерийн ухааны мэргэжилтнүүдэд маш их ач тустай тул практик мэдлэгтэй болох нь зүйтэй. Хиймэл оюун ухаан нь олон чиглэлийг дэмжих технологи тул тусдаа бие даасан лекц орохгүй; үүний оронд бид сүүлийн үеийн холбогдох AI хэрэгсэл, арга техникийг лекц бүрд шууд тусган оруулсан болно.

Энэхүү хичээлийг заах болсон [сэдэл, зорилгын тухай унших](/about/).

{% comment %}
# Registration

Sign up for the IAP 2026 class by filling out this [registration form](https://forms.gle/j2wMzi7qeiZmzEWy9).
{% endcomment %}

# Хичээлийн хөтөлбөр

{% comment %}
**Лекц**: [35-225](https://whereis.mit.edu/?go=35), 1:30--2:30pm (үл хамаарах тохиолдол: 1/16-ны Баасан гарагт 3--4pm)<br>
**Хэлэлцүүлэг**: [OSSU Discord](https://ossu.dev/#community) (асуулт асуухдаа Piazza шиг `#missing-semester-forum` сувгийг ашиглаж, багш болон бусад суралцагчидтай чатлахдаа `#missing-semester` сувгийг ашиглана уу)
{% endcomment %}

<ul>
{% assign lectures = site['2026'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-m/%-d/%y' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[no class]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

## Өмнөх жилүүдийн тусгай сэдвүүд

Бидний үзэх сэдвүүд жил бүр өөр өөр байдаг. Өмнөх жилүүдэд заасан нийт сэдвүүдийг сонирхож буй оюутнуудад зориулан 2026 онд ороогүй боловч өмнөх жилүүдэд үзсэн тусгай сэдвүүдийг энд онцлон харуулав.

{% comment %} pop to remove default "posts" collection {% endcomment %}
{% assign sorted_collections = site.collections | sort: 'label' | pop | reverse %}
<ul>
{% for collection in sorted_collections %}
    {% assign grouped_lectures = site[collection.label] | group_by: 'date' | sort: 'name' %}
    {% for group in grouped_lectures %}
        {% assign sorted_lectures = group.items | sort: 'order' %}
        {% for lecture in sorted_lectures %}
            {% if lecture.special == true %}
                <li>
                    <strong>{{ lecture.date | date: '%-m/%-d/%y' }}</strong>:
                    <a href="{{ lecture.url }}">{{ lecture.title }}</a>
                </li>
            {% endif %}
        {% endfor %}
    {% endfor %}
{% endfor %}
</ul>

{% comment %}
Lecture videos will be made available to MIT students immediately after lecture (via Panopto). The system has a limitation that only those with an MIT Kerberos can access the raw lecture videos. We are working on editing lecture videos and uploading them to YouTube. A couple have been uploaded already; we expect the rest to be uploaded by mid-February.

If you can't wait until January 2026, you can also take a look at the lectures
from the [previous offering of the course](/2020/), which covers many of the
same topics.
{% endcomment %}

# Ерөнхий мэдээлэл

**Багш нар**: Энэхүү хичээлийг [Anish](https://anish.io/), [Jon](https://thesquareplanet.com/), болон [Jose](https://josejg.com/) нар хамтран заана.<br>
**Асуулт**: Бидэнд [missing-semester@mit.edu](mailto:missing-semester@mit.edu) хаягаар имэйл илгээнэ үү.<br>
**Хэлэлцүүлэг**: [OSSU Discord](https://ossu.dev/#community) сувгаар явагдах бөгөөд асуулт асуухдаа Piazza шиг `#missing-semester-forum` сувгийг ашиглаж, багш болон бусад суралцагчидтай чатлахдаа `#missing-semester` сувгийг ашиглана уу.

# MIT-ээс гадуур

Бусад хүмүүст ч мөн эдгээр нөөц боломжууд ач тусаа өгнө гэж найдан бид энэхүү хичээлийг MIT-ээс гадуур хуваалцсан. Та дараах платформ дээрх нийтлэл, хэлэлцүүлгүүдийг үзэх боломжтой:

 - Hacker News ([2026](https://news.ycombinator.com/item?id=47124171), [2020](https://news.ycombinator.com/item?id=22226380), [2019](https://news.ycombinator.com/item?id=19078281))
 - Lobsters ([2026](https://lobste.rs/s/q4ykw7/missing_semester_your_cs_education_2026), [2020](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit), [2019](https://lobste.rs/s/h6157x/mit_hacker_tools_lecture_series_on))
 - r/learnprogramming ([2026](https://www.reddit.com/r/learnprogramming/comments/1r93yk6/the_missing_semester_of_your_cs_education_2026/), [2020](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/), [2019](https://www.reddit.com/r/learnprogramming/comments/an42uu/mit_hacker_tools_a_lecture_series_on_programmer/))
 - r/programming ([2020](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/), [2019](https://www.reddit.com/r/programming/comments/an3xki/mit_hacker_tools_a_lecture_series_on_programmer/))
 - X ([2026](https://x.com/anishathalye/status/2024521145777848588), [2020](https://twitter.com/jonhoo/status/1224383452591509507), [2019](https://x.com/jonhoo/status/1090323977766137858))
 - Bluesky ([2026](https://bsky.app/profile/jonhoo.eu/post/3mfa2bhyuj22i))
 - Mastodon ([2026](https://fosstodon.org/@jonhoo/116098318361854057))
 - LinkedIn ([2026](https://www.linkedin.com/posts/anishathalye_i-returned-to-mit-during-iap-january-term-activity-7430285026933522433-Ehr9))
 - YouTube ([2026](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L), [2020](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J), [2019](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuiujH1lpn8cA9dsyulbYRv))

# Орчуулгууд

{% comment %} keep these in alphabetical order {% endcomment %}

- [Arabic](https://missing-semester-ar.github.io/)
- [Bengali](https://missing-semester-bn.github.io/)
- [Chinese (Simplified)](https://missing-semester-cn.github.io/)
- [Chinese (Traditional, Taiwan)](https://missing-semester-tw.github.io/)
- [German](https://missing-semester-de.github.io/)
- [Italian](https://missing-semester-it.github.io/)
- [Japanese](https://missing-semester-jp.github.io/)
- [Kannada](https://missing-semester-kn.github.io/)
- [Korean](https://missing-semester-kr.github.io/)
- [Persian](https://missing-semester-fa.github.io/)
- [Portuguese](https://missing-semester-pt.github.io/)
- [Russian](https://missing-semester-rus.github.io/)
- [Serbian](https://netboxify.com/missing-semester/)
- [Spanish](https://missing-semester-esp.github.io/)
- [Swedish](https://den-saknade-terminen.l10n.se/)
- [Thai](https://missing-semester-th.github.io/)
- [Turkish](https://missing-semester-tr.github.io/)
- [Vietnamese](https://missing-semester-vn.github.io/)
- [Mongolian](https://missing-semester-mn.github.io/)

Тэмдэглэл: Эдгээр нь олон нийтийн хийсэн орчуулгын гадны холбоосууд бөгөөд бид тэдгээрийг хянаж шалгаагүй болно.

Та энэ хичээлийн тэмдэглэлийн орчуулгыг бэлтгэсэн үү? Жагсаалтад нэмүүлэхийн тулд анхны репозиторт [татах хүсэлт](https://github.com/missing-semester/missing-semester/pulls) илгээнэ үү!

## Талархал

Бид лекцийн видео бичлэгийг хийх боломжоор хангасан Элейн Мелло болон [MIT Open Learning](https://openlearning.mit.edu/)-д талархал илэрхийлье. Мөн [SIPB IAP 2026](https://sipb.mit.edu/iap/)-ийн хүрээнд энэхүү хичээлийг дэмжсэн Луис Турино болон [SIPB](https://sipb.mit.edu/)-д талархаж буйгаа илэрхийлье.

---

<div class="small center">
<p><a href="https://github.com/missing-semester-mn/missing-semester-mn.github.io">Эх код</a>.</p>
<p>Лиценз: CC BY-NC-SA.</p>
<p>Хувь нэмэр оруулах ба орчуулгын зааврыг <a href="/license/">эндээс</a> үзнэ үү.</p>
</div>
