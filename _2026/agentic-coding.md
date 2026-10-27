---
layout: lecture
title: "AI агенттай код бичих"
description: >
  Програм хангамжийн хөгжүүлэлтэд AI-ийн кодын агентуудыг хэрхэн үр дүнтэй ашиглах талаар үзнэ.
thumbnail: /static/assets/thumbnails/2026/lec7.png
date: 2026-01-21
ready: true
video:
  aspect: 56.25
  id: sTdz6PZoAnw
---

Кодын агент (coding agent) гэдэг нь чат маягийн AI загварт файл унших/бичих, вэбээс
хайх, шеллийн команд ажиллуулах зэрэг хэрэгслүүдийг нэмж өгсөн систем. Зарим
нь IDE дотор суудаг, зарим нь тусдаа командын мөрийн эсвэл GUI програм хэлбэртэй.
Эдгээр агент нэлээд бие даан ажилладаг болсон тул хөгжүүлэлтийн олон төрлийн
ажилд хэрэг болдог.

Энэ лекц [Хөгжүүлэлтийн орчин ба хэрэгслүүд](/2026/development-environment/)
лекцийн [AI-д суурилсан хөгжүүлэлт](/2026/development-environment/#ai-powered-development)
хэсгийг үргэлжлүүлнэ. Жишээ болгоод тэр хэсгийн кодыг цааш нь авч үзье:

```python
from urllib.request import urlopen

def download_contents(url: str) -> str:
    with urlopen(url) as response:
        return response.read().decode('utf-8')

def extract(content: str) -> list[str]:
    import re
    pattern = r'\[.*?\]\((.*?)\)'
    return re.findall(pattern, content)

print(extract(download_contents("https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/development-environment.md")))
```

Үүнийг кодын агентад ийм промптоор даалгаж болно:

```
Turn this into a proper command-line program, with argparse for argument parsing. Add type annotations, and make sure the program passes type checking.
```

Агент файлыг уншаад, хэрэгтэй засваруудаа хийж, эцэст нь төрлийн тэмдэглэгээ зөв
эсэхийг шалгахын тулд төрөл шалгагч ажиллуулна. Алдаа гаргаад шалгалтад унавал
дахин засаад дахин шалгана. Энэ бол агентуудын сайн хийдэг харьцангуй энгийн
ажил. Гэхдээ кодын агент системд нөлөөлж болох хэрэгсэл ажиллуулах эрхтэй тул
анхдагч байдлаараа хэрэгсэл ажиллуулах бүрдээ хэрэглэгчээс зөвшөөрөл асуудаг.

> Агент алдаа гаргавал --- жишээ нь `mypy` таны `$PATH` дээр шууд байхад агент
> `python -m mypy` гэж дуудахыг оролдвол --- текстээр зааж залруулж болно.

Кодын агенттай олон удаа харилцан ярилцаж болдог тул ажлыг явцын дунд чиглүүлж,
сайжруулж болно. Буруу зүг рүү явбал зогсоож ч болно. Энгийнээр хэлбэл дадлагажигч
удирдаж байгаа юм шиг: нарийн, уйтгартай ажлыг нь хийлгэж болно, гэхдээ чиглэл
өгөх, алдааг нь засах хэрэгтэй, эцсийн хариуцлага таны дээр үлдэнэ.

> Цааш туршихын тулд үүний дараа агентаас гарсан скриптийг ажиллуулахыг хүсээрэй.
> Гаралтыг нь хараад өөрчлөлт хүсэж болно (жишээ нь зөвхөн бүтэн (absolute) URL-уудыг
> үлдээ гэх мэт).

# AI загвар, агент хэрхэн ажилладаг вэ {#how-ai-models-and-agents-work}

Орчин үеийн [том хэлний загварууд (LLM)](https://en.wikipedia.org/wiki/Large_language_model)
болон агентын системийн дотоод ажиллагааг бүрэн тайлбарлах нь энэ хичээлийн
хүрээнээс гадуур. Гэхдээ хэдэн гол санааг ерөнхийд нь ойлгох нь энэ шинэ технологийг
үр дүнтэй *ашиглах*, хязгаарлалтыг нь мэдэхэд тусална.

LLM-ийг өгсөн промпт (оролтын тэмдэгт мөр)-оос ямар гаралт гарах магадлалын
тархалтыг загварчилдаг зүйл гэж ойлгож болно. LLM-ийг ажиллуулах (inference,
жишээ нь чат програм руу асуулт илгээх үед болдог зүйл) нь энэ тархалтаас
*түүвэр авах* үйлдэл. LLM-үүд оролт, гаралтын хамгийн их уртыг заадаг тогтсон
*контекстийн цонх* (context window)-той.

Чат, кодын агент зэрэг AI хэрэгслүүд энэ суурин дээр бүтсэн. Олон ээлжтэй
ярианы хувьд чат програм болон агентууд ярианы ээлжийг заагласан тусгай тэмдэгт
хэрэглэж, хэрэглэгч шинэ мессеж бичих бүрд ярианы түүхийг бүхэлд нь промпт болгон
LLM-ийг нэг удаа дууддаг. Хэрэгсэл ажиллуулдаг агентын хувьд систем LLM-ийн
зарим гаралтыг "хэрэгсэл дуудах хүсэлт" гэж үзээд ажиллуулж, үр дүнг нь промптод
нэмж загварт буцааж өгдөг (тиймээс хэрэгсэл дуудах, хариу авах бүрд LLM дахин
ажилладаг). Хэрэгсэл ашигладаг агентын гол санааг [200 мөр кодоор
хэрэгжүүлж](https://www.mihaileric.com/The-Emperor-Has-No-Clothes/) болно.

## Нууцлал

AI-д суурилсан ихэнх кодын хэрэгсэл анхдагч байдлаараа өгөгдлийг тань cloud
руу илгээдэг. Заримдаа агентын програм өөрөө таны машин дээр ажиллаж, зөвхөн
LLM-ийн тооцоолол cloud дээр хийгддэг. Бусад тохиолдолд програмын ихэнх хэсэг cloud
дээр ажиллах бөгөөд тэгвэл үйлчилгээ үзүүлэгч таны репогийн хуулбар болон AI
хэрэгсэлтэй хийсэн бүх ярианы түүхийг авах боломжтой болдог.

Нээлттэй эхийн AI кодын хэрэгслүүд, нээлттэй эхийн LLM-үүд бий, ажиллах нь
ч гайгүй, гэхдээ хаалттай загвартай эн тэнцүү биш. Мөн одоогийн байдлаар хамгийн
сүүлийн үеийн нээлттэй LLM-ийг өөрийн компьютер дээр ажиллуулах нь ихэнх хүнд
тоног төхөөрөмжийн хязгаараас болоод бараг боломжгүй.

# Хэрэглээ

Кодын агент олон төрлийн ажилд тусалж чадна. Хэдэн жишээ:

- **Шинэ боломж хэрэгжүүлэх.** Дээрх жишээн дээрхтэй адил кодын агентаар шинэ
  боломж хэрэгжүүлүүлж болно. Агентад сайн даалгавар өгөх нь одоогоор шинжлэх
  ухаан гэхээсээ илүү урлаг юм: агент юу хийхээ ойлгохоор хангалттай тодорхой
  байх ёстой (ядаж зөв чиглэлд эхлэх хэмжээнд, дараа нь засаж болно), гэхдээ
  бүх ажлыг өөрөө хийчихсэн юм шиг хэт нарийн байх шаардлагагүй. Тестэд суурилсан
  хөгжүүлэлт (TDD) их үр дүнтэй байж болно: тестүүдээ бичээд (эсвэл агентаар
  бичүүлээд), зөв эсэхийг нь шалгаад, дараа нь агентаар боломжийг хэрэгжүүлүүл.
  Загварууд байнга сайжирч байгаа тул тэдний юу чадахыг тухай бүрт нь дахин
  шалгаж байгаарай.
    > Missing Semester сайтын Tufte маягийн хажуугийн тэмдэглэлийг
      [нэмэхэд](https://github.com/missing-semester/missing-semester/pull/345)
      бид Claude Code ашигласан.

- **Алдаа засах.** Хөрвүүлэгч, линтер, төрөл шалгагч эсвэл тестүүд алдаа заавал
  агентаар засуулж болно, жишээ нь "fix the issues with mypy" гэх мэт промптоор.
  Загварууд ялангуяа хариу мэдээлэл авч давтан засах (feedback loop) үед их
  үр дүнтэй тул алдаа зааж буй шалгалтыг загвар өөрөө шууд ажиллуулж, бие даан
  засаж чаддаг байхаар тохируулахыг хичээгээрэй. Боломжгүй бол хариуг нь гараараа
  хуулж өгч болно.
    > Missing Semester репогийн [f552b55](https://github.com/missing-semester/missing-semester/commit/f552b5523462b22b8893a8404d2110c4e59613dd)
      коммит дээр бид Claude Code-д "Review the agentic coding lecture for typos
      and grammatical issues" гэж хэлээд, олсон асуудлуудыг нь засуулсан. Үр дүн
      нь [f1e1c41](https://github.com/missing-semester/missing-semester/commit/f1e1c417adba6b4149f7eef91ff5624de40dc637)
      коммитоор орсон.

- **Refactoring.** Кодын агентаар функцийн нэр солих гэх мэт энгийн ажлаас
  (үүнийг [кодын ухаалаг туслах](/2026/development-environment/#code-intelligence-and-language-servers)
  ч хийж чадна) эхлээд нэг хэсгийг тусдаа модуль болгон салгах гэх мэт нарийн
  refactoring хүртэл хийлгэж болно.
    > Missing Semester-т agentic coding-ийн хэсгийг тусдаа лекц болгон
      [салгахад](https://github.com/missing-semester/missing-semester/pull/344)
      бид Claude Code ашигласан.

- **Код хянах (code review).** Кодоо агентаар хянуулж болно. "review my latest
  changes that are not yet committed" гэх мэт энгийн заавар өгөхөд л болно. Pull
  request хянуулах бол, агент тань вэб хуудас уншиж чаддаг эсвэл [GitHub CLI](https://cli.github.com/)
  гэх мэт командын мөрийн хэрэгсэл суусан байвал "Review the pull request {холбоос}"
  гэж хэлэхэд агент үлдсэнийг нь өөрөө хийнэ.

- **Кодыг ойлгох.** Кодын сангийн талаар агентаас асууж болно. Шинэ төсөлтэй
  танилцахад их тус болно.

- **Шелл маягаар хэрэглэх.** Агентаас тодорхой ажлыг тодорхой хэрэгслээр хийлгэж,
  шеллийн командыг энгийн хэлээр дуудаж болно. Жишээ нь "use the find command
  to find all files older than 30 days" эсвэл "use mogrify to resize all the jpgs
  to 50% of their original size".

- **Vibe coding.** Агентууд өөрөө нэг ч мөр код бичихгүйгээр зарим програмыг бүтнээр
  нь хийлгэж болохуйц сайн болсон.
    > [Энд](https://github.com/cleanlab/office-presence-dashboard) багш нарын нэгний
      бүхэлд нь vibe coding-оор хийсэн бодит төсөл байна.

# Ахисан түвшний агент

Энд кодын агентуудын хэрэглээний ахисан түвшний хэдэн хэв маяг, боломжуудыг
товч танилцуулъя.

- **Дахин ашиглах промпт.** Дахин хэрэглэх промпт эсвэл загвар үүсгэх. Жишээ нь
  кодыг тодорхой байдлаар хянах нарийн промпт бичээд хадгалж болно.
    > Агентын хэрэгслүүд их хурдан өөрчлөгдөж байна. Зарим хэрэгсэлд дахин ашиглах
      промптын тусдаа боломж аль хэдийн хуучирсан. Жишээ нь Codex, Claude Code
      хоёрт тэдгээрийг [skills](https://code.claude.com/docs/en/skills) гэдэг
      боломжоор [сольсон](https://developers.openai.com/codex/custom-prompts).

- **Зэрэгцээ агент.** Кодын агент удаан ажиллаж магадгүй: даалгавар өгөхөд асуудлыг
  шийдэх гэж хэдэн арван минут ажиллаж болно. Агентын олон хувийг зэрэг ажиллуулж
  болно. Нэг ажил дээр (LLM санамсаргүй шинжтэй тул нэг ажлыг хэд хэдэн удаа хийлгээд
  хамгийн сайныг нь сонгож болно) эсвэл өөр өөр ажил дээр (жишээ нь хоорондоо
  давхцахгүй хоёр боломжийг зэрэг хэрэгжүүлэх) ажиллуулж болно. Агентуудын
  өөрчлөлт бие биедээ саад болохгүйн тулд [хувилбарын хяналт](/2026/version-control/)
  лекцээр үзсэн [git worktree](https://git-scm.com/docs/git-worktree)-г ашиглаж болно.

- **MCP.** Model Context Protocol буюу MCP бол кодын агентаа бусад хэрэгсэлтэй
  холбох нээлттэй протокол. Жишээ нь энэ [Notion MCP сервер](https://github.com/makenotion/notion-mcp-server)
  агентад тань Notion-ий баримт унших/бичих боломж олгож, "Notion-д бичсэн техникийн
  даалгаврыг уншаад, хэрэгжүүлэх төлөвлөгөөг Notion-д шинэ хуудас болгон бич, дараа
  нь прототипийг хий" гэх мэт ажил хийлгэх боломжтой болгоно. MCP-үүдийг
  [Pulse](https://www.pulsemcp.com/servers), [Glama](https://glama.ai/mcp/servers)
  гэх мэт жагсаалтуудаас олж болно.

- **Контекстийг удирдах.** [Дээр дурдсанчлан](#how-ai-models-and-agents-work)
  кодын агентуудын цаана ажилладаг LLM-үүдийн *контекстийн цонх* хязгаартай.
  Агентыг үр дүнтэй ашиглахын тулд контекстоо зөв удирдах хэрэгтэй. Агентад
  хэрэгтэй мэдээлэл нь байх ёстой, гэхдээ хэрэггүй мэдээллээр цонхыг дүүргэх,
  загварын гүйцэтгэлийг муутгахаас зайлсхийх хэрэгтэй (контекст томрох тусам,
  цонхонд багтаж байсан ч гүйцэтгэл буурах хандлагатай). Агентын системүүд
  контекстыг автоматаар тодорхой хэмжээнд удирддаг ч ихэнх хяналт хэрэглэгчийн
  гарт үлддэг.
    - **Контекстоо цэвэрлэх.** Хамгийн энгийн хяналт: кодын агентууд контекстоо
      цэвэрлэх буюу шинэ яриа эхлүүлэх боломжтой. Хамааралгүй шинэ асуулт бүрд
      үүнийг хийх нь зүйтэй.
    - **Яриаг ухраах.** Зарим агент ярианы түүхээс ухрах боломжтой. Агентыг өөр
      зүг рүү чиглүүлэх гэж нэмэлт мессеж бичихээс илүү буцаах (undo) нь контекстыг
      илүү цэвэр байлгадаг.
    - **Хураангуйлах (compaction).** Хязгааргүй урт яриа хийхийн тулд агентууд
      контекстоо хураангуйлж чаддаг: ярианы түүх хэт урт болбол LLM-ээр эхний
      хэсгийг нь хураангуйлаад түүхийг тэр хураангуйгаар солино. Зарим агент
      хэрэглэгч хүссэн үедээ үүнийг хийлгэх боломж олгодог.
    - **llms.txt.** `/llms.txt` бол LLM-д ажиллах үед нь хэрэгтэй баримт бичгийг
      тавих [стандарт](https://llmstxt.org/) байршил. Бүтээгдэхүүн (жишээ нь
      [cursor.com/llms.txt](https://cursor.com/llms.txt)), сан (жишээ нь
      [ai.pydantic.dev/llms.txt](https://ai.pydantic.dev/llms.txt)), API (жишээ нь
      [apify.com/llms.txt](https://apify.com/llms.txt)) зэрэгт хөгжүүлэлтэд хэрэгтэй
      `llms.txt` файлууд бий. Ийм баримт бичиг тэмдэгт бүрт ногдох мэдээлэл ихтэй тул
      агентаар HTML хуудас уншуулахаас контекстыг хамаагүй хэмнэдэг. Ялангуяа агентын
      мэдлэгийн хугацаанаас хойш гарсан хамаарлуудыг хэрэглэхэд гадны баримт бичиг их
      хэрэгтэй.
    - **AGENTS.md.** Ихэнх кодын агент [AGENTS.md](https://agents.md/) эсвэл түүнтэй
      адил файлыг (жишээ нь Claude Code `CLAUDE.md` хайдаг) өөртөө зориулсан
      заавар болгон дэмждэг. Агент эхлэхдээ `AGENTS.md`-ийн агуулгыг бүтнээр нь
      контекстдоо оруулдаг. Үүгээр сесс бүрд хэрэгтэй нийтлэг зааврыг өгч болно
      (жишээ нь кодоо өөрчилсний дараа заавал төрөл шалгагч ажиллуул, тестийг ингэж
      ажиллуулна, эдгээр гадны баримт бичгийг харж болно гэх мэт). Зарим агент энэ
      файлыг автоматаар үүсгэж чаддаг (жишээ нь Claude Code-ийн `/init` команд).
      Бодит төслийн `AGENTS.md`-ийн жишээг [эндээс](https://github.com/pydantic/pydantic-ai/blob/main/CLAUDE.md)
      хараарай.
    - **Skills.** `AGENTS.md`-ийн агуулга үргэлж агентын контекстэд бүтнээрээ
      ачаалагддаг. _Skills_ контекстыг хэмнэх нэмэлт шат болдог: агентад товч
      тайлбартай skill-үүдийн жагсаалт өгөхөд агент хэрэгтэй үедээ тухайн skill-ийг
      "нээж" контекстдоо ачаална.
    - **Дэд агент (subagent).** Зарим кодын агент тодорхой ажилд зориулсан дэд
      агент тодорхойлох боломж олгодог. Үндсэн агент тодорхой ажлыг хийлгэхээр дэд
      агентыг дуудна. Ингэснээр контекстыг илүү үр ашигтай удирдана.
      Үндсэн агентын контекст дэд агентын харсан бүх зүйлээр дүүрэхгүй, дэд агент
      ч зөвхөн өөрийн ажилд хэрэгтэй контекстыг л авна. Жишээ нь вэбээс судлах дэд
      агент: үндсэн агент асуулт өгөхөд дэд агент вэбээс хайж, хуудсуудыг уншиж
      шинжлээд, зөвхөн хариуг нь буцаана. Ингэснээр үндсэн агентын контекст татсан
      вэб хуудсуудаар дүүрэхгүй, дэд агентын контекст ч үндсэн агентын ярианы
      түүхээр дүүрэхгүй.

Промпт бичих шаардлагатай нарийн боломжуудын (skill, дэд агент) эхний хувилбарыг
LLM-ээр бичүүлж болно. Зарим агент үүнийг өөрөө дэмждэг. Жишээ нь Claude Code
богино промптоос дэд агент үүсгэж чадна (`/agents` командаар). Ийм промптоор
дэд агент үүсгэж үзээрэй:

```
A Python code checking agent that uses `mypy` and `ruff` to type-check, lint, and format *check* any files that have been modified from the last git commit.
```

Дараа нь үндсэн агентдаа "use the code checker subagent" гэж хэлж дэд агентыг
дуудаж болно. Мөн тодорхой нөхцөлд (жишээ нь Python файл өөрчлөгдсөний дараа) дэд
агентыг автоматаар дуудахаар үндсэн агентаа тохируулж болно.

# Юунаас болгоомжлох вэ

AI хэрэгслүүд алдаа гаргана. Тэд дараагийн тэмдэгтийг магадлалаар таамагладаг LLM
дээр суурилсан. Хүн шиг "ухаантай" биш. AI-ийн гаргасан үр дүнг зөв эсэх, аюулгүй
байдлын алдаагүй эсэхийг байнга шалгаж байгаарай. Заримдаа кодыг шалгах нь
өөрөө бичихээс ч хэцүү байдаг тул чухал кодоо гараараа бичихийг бодоорой. AI
мухардалд орж, таныг ч төөрөгдүүлж болно: дебаг хийхдээ эцэс төгсгөлгүй тойрог
(debugging spiral) руу орохоос болгоомжил. AI-ийг туслах хэрэгсэл гэж хар, түүнд
хэт найдаж, өнгөц ойлголттой үлдэхээс сэргийл. AI одоохондоо хийж чадахгүй
програмчлалын ажил маш олон бий. Тооцооллын сэтгэлгээ үнэ цэнэтэй хэвээр.

# Санал болгох програмууд

Олон IDE болон AI кодын өргөтгөлүүд кодын агенттай болсон ([Хөгжүүлэлтийн
орчин](/2026/development-environment/) лекцийн жагсаалтыг үзээрэй). Бусад алдартай
кодын агент гэвэл Anthropic-ийн [Claude Code](https://www.claude.com/product/claude-code),
OpenAI-ийн [Codex](https://openai.com/codex/), нээлттэй эхийн [opencode](https://github.com/anomalyco/opencode).

# Дасгал ажил

1. Нэг ижил програмчлалын ажлыг дөрвөн удаа хийж харьцуулаарай: гараараа код
   бичих, AI-ийн автомат нөхөлт ашиглах, кодын доторх чат ашиглах, агент ашиглах.
   Ажиллаж буй төслийнхөө жижиг боломж хамгийн тохиромжтой. Өөр санаа хэрэгтэй бол
   GitHub дээрх нээлттэй эхийн төслүүдийн "good first issue" маягийн ажлууд, эсвэл
   [Advent of Code](https://adventofcode.com/), [LeetCode](https://leetcode.com/)-ийн
   бодлогуудыг авч болно.
1. Танихгүй код ойлгоход кодын агент ашиглаарай. Санаа тавьдаг төслийнхөө алдааг
   засах эсвэл шинэ боломж нэмэх хүрээнд хийвэл хамгийн сайн. Тийм төсөл байхгүй
   бол [opencode](https://github.com/anomalyco/opencode) агентын аюулгүй байдалтай
   холбоотой боломжууд хэрхэн ажилладгийг агентаар ойлгуулж үзээрэй.
1. Гараараа нэг ч мөр код бичилгүйгээр жижиг програмыг бүтнээр нь vibe coding-оор
   хийгээрэй.
1. Сонгосон кодын агентдаа `AGENTS.md` (эсвэл `CLAUDE.md` гэх мэт), skill (жишээ нь
   [Claude Code-ийн skill](https://code.claude.com/docs/en/skills) эсвэл
   [Codex-ийн skill](https://developers.openai.com/codex/skills/)), дэд агент (жишээ нь
   [Claude Code-ийн subagent](https://code.claude.com/docs/en/sub-agents)) үүсгэж
   туршаарай. Аль нэгийг нь хэзээ сонгох талаар бодоорой. Таны агент эдгээрийн
   заримыг дэмжихгүй байж болно; тэгвэл алгасах эсвэл дэмждэг өөр агент дээр туршаарай.
1. [Кодын чанар](/2026/code-quality/) лекцийн Markdown жагсаалтын тэмдэгт солих
   (regex-ийн) дасгалыг кодын агентаар хийлгээрэй. Агент файлыг шууд засаж байна
   уу? Ийм ажлыг файлыг шууд засаж хийхийн сул тал, хязгаар нь юу вэ? Файлыг
   шууд засахгүйгээр хийлгэхийн тулд агентад ямар промпт өгөх вэ? Сэжүүр: [эхний
   лекцээр](/2026/course-shell/) үзсэн командын мөрийн хэрэгслүүдийн нэгийг
   ашиглахыг хүсээрэй.
1. Ихэнх кодын агент "yolo горим" буюу зөвшөөрөл асуухгүй горимтой (жишээ нь Claude
   Code-ийн `--dangerously-skip-permissions`). Үүнийг шууд ажиллуулах аюултай, харин
   агентыг виртуал машин эсвэл контейнер гэх мэт тусгаарлагдсан орчинд ажиллуулж,
   тэнд бие даах горимыг асаах нь илүү зөв. Үүнийг өөрийн машин дээрээ тохируулаад
   ажиллуулаарай. [Claude Code devcontainers](https://code.claude.com/docs/en/devcontainer),
   [Docker Sandboxes / Claude Code](https://docs.docker.com/ai/sandboxes/agents/claude-code/)
   зэрэг баримт бичиг хэрэг болж магадгүй. Тохируулах хэд хэдэн арга бий.

{% comment %}
> In mathematical notation, the LLM models the probability distribution $\pi_\theta$ of completions $y$ conditioned on prompts $x$, and we sample from this distribution: $\hat{y} \sim \pi_\theta(\cdot \mid x)$.
{% endcomment %}

{%- comment %}
No need to demo this, since the intro of a lecture was a small demo of adding a new feature.
{% endcomment %}

{%- comment %}
Demo a coding agent fixing the bug in https://github.com/anishathalye/dotbot/commit/cef40c902ef0f52f484153413142b5154bbc5e99.

Write the failing tests to demo the bug, and then ask the agent to fix. Prepped in branch demo-bugfix.

Can run the failing test with:

    hatch test tests/test_cli.py::test_issue_357

Can prompt coding agent with:

    There is a bug I wrote a failing test for, you can repro it with `hatch test tests/test_cli.py::test_issue_357`. Fix the bug.

Get it to commit the changes.
{% endcomment %}

{%- comment %}
Show usage in Missing Semester, point out that the agent did make some mistakes.
{% endcomment %}

{%- comment %}
In Porcupine repo, prompt agent with:

    Review this PR: https://github.com/anishathalye/porcupine/pull/39
{% endcomment %}

{%- comment %}
Some prompts to try in the missing-semester repo:

    How do I run this site locally?

    How are the social preview cards implemented?
{% endcomment %}

{%- comment %}
In Dotbot repo, prompt agent with:

    Use the ag command to find all Python renaming imports
{% endcomment %}

{%- comment %}
In missing-semester repo, prompt agent with:

    Make this site look retro.
{% endcomment %}

{%- comment %}
Make up a quick demo.
{% endcomment %}

{%- comment %}
Show `/compact` in Claude Code, show full summary.
{% endcomment %}

{%- comment %}
Side-by-side comparison in an empty repo (on Desktop or some other self-contained place, with `git init` run in it):

    Write a single-file Python program example in demo.py using semlib to sort "Ilya Sutskever", "Soumith Chintala", and "Donald Knuth" in terms of their fame as AI researchers.

    Write a single-file Python program example in demo.py using semlib to sort "Ilya Sutskever", "Soumith Chintala", and "Donald Knuth" in terms of their fame as AI researchers. See https://semlib.anish.io/llms.txt. Follow links to Markdown versions of any pages linked in llms.txt files.

Not sure why the agent doesn't do this by default. You'd probably put that last sentence in a CLAUDE.md file.
{% endcomment %}

{%- comment %}
Dotbot example, CLAUDE.md that includes @DEVELOPMENT.md and says to always run the type checker and code formatter after making any changes to Python code.

Example prompt, off of master:

    Remove the "--version" command-line flag.

This is something that'll be fast, for demonstration purposes.
{% endcomment %}
