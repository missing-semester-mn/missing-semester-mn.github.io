---
layout: lecture
title: "Вэб ба хөтчүүд"
presenter: Jose
date: 2019-01-31
order: 1
video:
  aspect: 62.5
  id: XpZO3S8odec
special: true
---

Терминалаас гадна хөтөч бол таны хамгийн их цаг зарцуулах хэрэгслүүдийн нэг.
Тиймээс түүнийг үр дүнтэй хэрэглэж сурах нь зүйтэй.

## Товчлол (Shortcuts)

Хөтөч дээр хулганаар товшоод байх нь ихэвчлэн хамгийн хурдан арга биш. Түгээмэл
товчлолуудыг сурвал урт хугацаандаа их өгөөжөө өгнө.

- Холбоос дээр `Хулганы дунд товчлуураар дарах` - шинэ табд нээнэ.
- `Ctrl+T` - Шинэ таб нээнэ.
- `Ctrl+Shift+T` - Саяхан хаасан табыг дахин нээнэ.
- `Ctrl+L` - Хаягийн мөрийн агуулгыг сонгоно.
- `Ctrl+F` - Хуудас дотроос хайна. Байнга хэрэглэдэг бол хайлтад regex дэмждэг
  өргөтгөл хэрэг болж магадгүй.

## Хайлтын операторууд (Search operators)

Google, DuckDuckGo зэрэг хайлтын системүүд илүү нарийн хайлт хийх хайлтын
оператортой:

- `"bar foo"` - "bar foo"-г яг таг тааруулна.
- `foo site:bar.com` - foo-г зөвхөн bar.com дотроос хайна.
- `foo -bar` - bar орсон үр дүнг хасна.
- `foobar filetype:pdf` - тухайн өргөтгөлтэй файлуудыг хайна.
- `(foo|bar)` - foo ЭСВЭЛ bar орсон үр дүнг хайна.

Алдартай хайлтын системүүдийн илүү бүрэн жагсаалт:
[Google](https://ahrefs.com/blog/google-advanced-search-operators/),
[DuckDuckGo](https://duck.co/help/results/syntax).

## Хаягийн мөр (Searchbar)

Хаягийн мөр ч хүчтэй хэрэгсэл. Ихэнх хөтөч сайтуудын хайлтыг автоматаар таньж
хадгалдаг. Тэдний түлхүүр үгийг засаж болно:

- Google Chrome-д [chrome://settings/searchEngines](chrome://settings/searchEngines)
  дотор бий.
- Firefox-д [about:preferences#search](about:preferences#search) дотор бий.

Жишээ нь `y ХАЙХ ҮГС` гэж бичээд шууд YouTube дотроос хайдаг болгож болно.

Мөн өөрийн домэйнтэй бол домэйн бүртгэгчээрээ дэд домэйнийг өөр хаяг руу
чиглүүлж болно. Жишээ нь би `https://ht.josejg.com`-ийг энэ хичээлийн сайт руу
чиглүүлсэн. Тэгэхээр `ht.` гэж л бичихэд хаягийн мөр автоматаар нөхнө. Энэ
аргын бас нэг давуу тал нь хавчуургаас (bookmark) ялгаатай нь бүх хөтөч дээр
ажилладаг.

## Нууцлалын өргөтгөлүүд (Privacy extensions)

Өнөөдөр вэбээр аялах нь зар сурталчилгаанаас болж залхаамжтай, мөрдөгчөөс
(tracker) болж хувийн орон зайд халддаг болсон. Сайн зар хаагч ихэнх зарыг
хаахаас гадна нийтлэг хар жагсаалтад орсон сэжигтэй, хортой сайтуудыг ч хаана.
Хүсэлтийн тоог багасгаснаар заримдаа хуудас хурдан ачаалагдана. Хэдэн санал:

- **uBlock Origin** ([Chrome](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)):
  урьдчилан тогтоосон дүрмээр зар, мөрдөгчийг хаана. Тохиргоонд асаалттай хар
  жагсаалтуудыг харж, бүс нутаг, хэрэглээндээ тааруулж нэмж асаагаарай.
  [Интернэтэд байгаа шүүлтүүрүүдийг](https://github.com/gorhill/uBlock/wiki/Filter-lists-from-around-the-web)
  ч суулгаж болно.
- **[Privacy Badger](https://privacybadger.org/)**: мөрдөгчийг автоматаар
  илрүүлж хаана. Жишээ нь таныг сайтаас сайт руу шилжихэд зар сурталчилгааны
  компаниуд аль сайтад орсныг тань мөрдөж, таны тухай профайл бүрдүүлдэг.
- **[HTTPS Everywhere](https://www.eff.org/https-everywhere)**: боломжтой бол
  сайтын HTTPS хувилбар руу автоматаар шилжүүлдэг гайхалтай өргөтгөл.

Ийм төрлийн өөр өргөтгөлүүдийг [эндээс](https://www.privacytools.io/privacy-browser-addons/)
олоорой.

## Загвар өөрчлөх (Style customization)

Хөтөч бол *таны машин дээр* ажиллаж буй нэг л програм, тиймээс юу харуулах, яаж
ажиллахыг ихэвчлэн эцсийн дүндээ та шийднэ. Үүний нэг жишээ бол өөрийн загвар.
Хөтөч хуудсыг ямар загвартай харуулахаа Cascading Style Sheets буюу CSS-ээр
тодорхойлдог.

Хөтчийн inspect хэрэгслээр сайтын эх кодыг нээж, агуулга, загварыг түр өөрчилж
болно (вэб хуудасны дэлгэцийн зурагт хэзээ ч итгэж болохгүйн нэг шалтгаан энэ).

Загвараа байнгын болгож өөрчлөх бол өргөтгөл хэрэгтэй. Бидний зөвлөх нь
**[Stylus](https://github.com/openstyles/stylus)**
([Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), [Chrome](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne?hl=en)).

Жишээ нь хичээлийн сайтад ийм загвар бичиж болно:

```css
body {
    background-color: #2d2d2d;
    color: #eee;
    font-family: Fira Code;
    font-size: 16pt;
}

a:link {
    text-decoration: none;
    color: #0a0;
}
```

Мөн Stylus бусад хэрэглэгчдийн бичиж [userstyles.org](https://userstyles.org/)
дээр нийтэлсэн загваруудыг олж чадна. Жишээ нь ихэнх алдартай сайтад нэг буюу
хэд хэдэн бараан өнгөний загвар бий. Дашрамд, Stylish-ийг бүү хэрэглээрэй,
хэрэглэгчийн өгөгдлийг задруулдаг нь тогтоогдсон
([дэлгэрэнгүй](https://arstechnica.com/information-technology/2018/07/stylish-extension-with-2m-downloads-banished-for-tracking-every-site-visit/)).

## Ажиллагааг өөрчлөх (Functionality Customization)

Загварыг өөрчилдөгтэй адил сайтын ажиллагааг ч өөрийн JavaScript бичиж,
[Tampermonkey](https://tampermonkey.net/) зэрэг өргөтгөлөөр ажиллуулж өөрчилж
болно.

Жишээ нь дараах скрипт J, K товчоор Vim шиг дээш доош гүйлгэдэг болгоно:

```js
// ==UserScript==
// @name         VIM HT
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Vim JK for our website
// @author       You
// @match        https://hacker-tools.github.io/*
// @grant        none
// ==/UserScript==


(function() {
    'use strict';

    window.onkeyup = function(e) {
        var key = e.keyCode ? e.keyCode : e.which;

        if (key == 74) { // J нь 74-р товчлуур
            window.scrollBy(0,500);;
        }else if (key == 75) { // K нь 75-р товчлуур
            window.scrollBy(0,-500);;
        }
    }
})();
```

[OpenUserJS](https://openuserjs.org/), [Greasy Fork](https://greasyfork.org/en)
зэрэг скриптийн сангууд ч бий. Гэхдээ анхаараарай: бусдын бичсэн скрипт суулгах
маш аюултай, тэд бараг юу ч хийж чадна, жишээ нь зээлийн картын дугаарыг тань
хулгайлна. Бүтэн скриптийг өөрөө уншиж, юу хийдгийг ойлгоод, сэжигтэй юм
хийхгүй гэдэгт бүрэн итгэлтэй болохоос нааш хэзээ ч бүү суулгаарай. Уншиж
болохооргүй шахсан (minified) эсвэл зориуд ойлгомжгүй болгосон (obfuscated) код
агуулсан скриптийг хэзээ ч бүү суулгаарай!

## Вэб API (Web APIs)

Вэб үйлчилгээнүүд вэб хүсэлтээр харьцаж болох програмын интерфейс буюу вэб API
санал болгох нь улам түгээмэл болж байна. Энэ сэдвийн дэлгэрэнгүй танилцуулгыг
[эндээс](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)
уншаарай. [Нийтэд нээлттэй API](https://github.com/toddmotto/public-apis) олон
бий. Вэб API олон зүйлд хэрэгтэй:

- **Мэдээлэл авах (Retrieval)**. Вэб API газрын зураг, цаг агаар, нийтийн IP
  хаяг тань гэх мэт мэдээллийг амархан өгнө. Жишээ нь `curl ipinfo.io` нийтийн
  IP, бүс нутаг, байршил гэх мэт мэдээлэлтэй JSON объект буцаана. Зөв задалбал
  эдгээрийг командын мөрийн хэрэгслүүдтэй ч холбож болно. Дараах bash функц
  Google-ийн автомат нөхөлтийн API-тай харьцаж эхний арван үр дүнг буцаана:

```bash
function c() {
    url='https://www.google.com/complete/search?client=hp&hl=en&xhr=t'
    # Санамж: UTF-8 өгөгдөл авахын тулд user-agent-ийг заавал зааж өгөх ёстой!
    curl -H 'user-agent: Mozilla/5.0' -sSG --data-urlencode "q=$*" "$url" |
        jq -r ".[1][][0]" |
        sed 's,</\?b>,,g'
}
```

- **Үйлдэл хийлгэх (Interaction)**. Вэб API-аар үйлдэл өдөөж ч болно. Ихэвчлэн
  үйлчилгээнээс авсан нэвтрэх токен хэрэгтэй. Жишээ нь дараах команд:
  `curl -X POST -H 'Content-type: application/json' --data '{"text":"Hello, World!"}' "https://hooks.slack.com/services/$SLACK_TOKEN"` Slack
  суваг руу `Hello, World!` гэсэн мессеж илгээнэ.

- **Холбох (Piping)**. Вэб API-тай зарим үйлчилгээ их алдартай тул тэднийг
  хооронд нь холбосон түгээмэл шийдлүүд бэлэн, сервер нь хүртэл хамт ирдэг.
  [If This Then That](https://ifttt.com/), [Zapier](https://zapier.com/) яг ийм
  үйлчилгээ.

## Вэб автоматжуулалт (Web Automation)

Заримдаа вэб API хүрэлцэхгүй. Зөвхөн унших бол `pup` шиг HTML задлагч, эсвэл
Python-ы BeautifulSoup шиг сан хэрэглэж болно. Харин хуудастай харилцах эсвэл
JavaScript ажиллуулах хэрэгтэй бол эдгээр нь хүрэлцэхгүй, тэр үед WebDriver
хэрэглэнэ.

Жишээ нь дараах скрипт сайтын хаягийг бичиж оруулах үйлдлийг дуурайж, Wayback
Machine-д тухайн URL-ыг хадгална:

```python
from selenium.webdriver import Firefox
from selenium.webdriver.common.keys import Keys


def snapshot_wayback(driver, url):

    driver.get("https://web.archive.org/")
    elem = driver.find_element_by_class_name('web-save-url-input')
    elem.clear()
    elem.send_keys(url)
    elem.send_keys(Keys.RETURN)
    driver.close()


driver = Firefox()
url = 'https://hacker-tools.github.io'
snapshot_wayback(driver, url)
```

## Дасгал ажил

1. Хөтөч дээрээ байнга хэрэглэдэг хайлтын системийнхээ түлхүүр үгийг засаарай.

1. Дээрх өргөтгөлүүдийг суулгаарай. uBlock Origin/Privacy Badger-ийг тодорхой
   сайт дээр хэрхэн унтраахыг олоорой. Ямар ялгаа харагдаж байна? YouTube шиг
   зар ихтэй сайт дээр туршаарай.

1. Stylus суулгаад дээрх CSS-ээр хичээлийн сайтад өөрийн загвар бичээрэй.
   Програмчлалд түгээмэл тэмдэгтүүд: `=   ==   ===   >=   =>   ++   /=   ~=`.
   Фонтыг Fira Code болгоход тэд юу болж байна? Илүүг мэдэх бол "programming
   font ligatures" гэж хайгаарай.

1. Өөрийн хот/бүс нутгийн цаг агаарыг авах вэб API олоорой.

1. [Selenium](https://www.selenium.dev/documentation/) шиг WebDriver програмаар
   хөтөч дээрээ байнга гараар хийдэг давтагдах ажлаа автоматжуулаарай.
