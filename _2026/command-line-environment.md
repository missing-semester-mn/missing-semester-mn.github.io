---
layout: lecture
title: "Командын мөрийн орчин"
description: >
  Командын мөрийн програмууд яаж ажилладгийг үзнэ: аргумент, оролт/гаралтын урсгал, орчны хувьсагч, гаралтын код, сигнал, мөн SSH-ээр алсын сервертэй ажиллах.
thumbnail: /static/assets/thumbnails/2026/lec2.png
date: 2026-01-13
ready: true
video:
  aspect: 56.25
  id: ccBGsPedE9Q
---

Өмнөх лекцээр үзсэнчлэн шелл бол зүгээр нэг програм ажиллуулагч биш. Ихэнх шелл
өөрөө бүрэн хэмжээний скриптийн хэлтэй, дээр нь олон нийтлэг хэв загвар, хийсвэрлэл
агуулдаг. Гэхдээ Python, Ruby шиг ерөнхий зориулалтын хэлнүүдээс ялгаатай нь шелл
скриптийн төвд програм ажиллуулах, тэдгээрийг хооронд нь эвтэйхэн холбох санаа
байдаг.

Шелл скрипт бичихэд *тогтсон дүрэм* (convention) их чухал. Командын мөрийн програм
шелл орчинд эвтэйхэн ажиллахын тулд хүмүүсийн хүлээдэг хэдэн нийтлэг дүрмийг
дагадаг. Одоо командын мөрийн програмууд яаж ажилладаг, яаж тохируулагддаг, шеллтэй
яаж "сайн иргэн" шиг харилцдагийг үзье.

# Командын мөрийн интерфейс

Ихэнх програмчлалын хэл дээр функц ингэж харагддаг:

```
def add(x: int, y: int) -> int:
    return x + y
```

Энд оролт, гаралт нь шууд харагдаж байна. Шелл скрипт харин эхлээд харахад арай
өөр мэдрэмж төрүүлнэ:

```shell
#!/usr/bin/env bash

if [[ -f $1 ]]; then
    echo "Target file already exists"
    exit 1
else
    if $DEBUG; then
        grep 'error' - | tee $1
    else
        grep 'error' - > $1
    fi
    exit 0
fi
```

Ийм скрипт дотор юу болж байгааг ойлгохын тулд шелл програмууд хоорондоо болон
шелл орчинтой харилцахдаа ашигладаг хэдэн үндсэн ойлголтыг эхлээд авъя:

- Аргумент
- Урсгал (stream)
- Орчны хувьсагч (environment variable)
- Гаралтын код (exit code)
- Сигнал

## Аргумент

Шелл програм ажиллахдаа аргументын жагсаалт авдаг. Шеллийн хувьд аргументууд
зүгээр л тэмдэгт мөр; харин тэдгээрийг яаж ойлгох нь тухайн програмаас хамаарна.
Жишээ нь `ls -l folder/` гэж ажиллуулахад шелл `/bin/ls`-ийг `['-l', 'folder/']`
гэсэн аргументуудтай дуудна.

Шелл скрипт дотор эдгээрийг тусгай синтаксаар авна. Эхний аргумент `$1`, хоёр дахь
нь `$2` гэх мэтээр `$9` хүртэл явна. Бүх аргументыг жагсаалт байдлаар авах бол
`$@`, аргументын тоо хэрэгтэй бол `$#` ашиглана. Ажиллаж байгаа скрипт/програмын
нэр `$0` дээр байдаг.

Ихэнх програмын аргумент нь *флаг* болон энгийн тэмдэгт мөрүүдийн холимог байдаг. Флагийг нэг зураас (`-`) эсвэл давхар зураасаар (`--`) эхэлдгээр нь таньж болно. Флаг ихэвчлэн заавал биш бөгөөд програмын үйлдлийг өөрчлөхөд хэрэглэгддэг. Жишээ нь `ls -l` нь `ls` гаралтаа ямар форматаар хэвлэхийг өөрчилдөг.

`--all` шиг урт нэртэй давхар зураастай флаг, `-a` шиг ихэвчлэн ганц үсэгтэй нэг зураастай флаг хоёуланг нь харна. Нэг тохиргоог хоёр хэлбэрээр өгч болдог: `ls -a`, `ls --all` хоёр адилхан. Нэг зураастай флагуудыг ихэвчлэн нийлүүлж бичдэг тул `ls -l -a`, `ls -la` хоёр ч адилхан. Флагийн дараалал ч ихэвчлэн хамаагүй: `ls -la`, `ls -al` ижил үр дүн өгнө. Зарим флаг маш түгээмэл тул шеллд дасах тусам гараараа шууд бичдэг болно (жишээ нь `--help`, `--verbose`, `--version`).

> Флаг бол шеллийн тогтсон дүрмийн сайн жишээ. Шелл хэл програмаас заавал `-` эсвэл `--` хэрэглэ гэж шаарддаггүй. `myprogram +myoption myfile` гэсэн синтакстай програм бичихэд юу ч саадгүй, гэхдээ хүмүүс зураастай хэлбэрийг хүлээдэг тул бусдыг төөрөлдүүлнэ.
> Бодит байдал дээр ихэнх програмчлалын хэлэнд командын мөрийн флаг задалдаг бэлэн сан бий (жишээ нь Python-ы `argparse`).

Командын мөрийн програмуудын бас нэг нийтлэг дүрэм бол нэг төрлийн олон аргумент хүлээн авах. Ингэж өгвөл команд аргумент бүр дээр ижил үйлдлийг хийнэ.

```shell
mkdir src
mkdir docs
# is equivalent to
mkdir src docs
```

Энэ товчлол эхэндээ хэрэггүй юм шиг санагдаж магадгүй, гэхдээ *glob*-той хослоход жинхэнэ хүч нь гарч ирдэг. Glob гэдэг нь програм дуудагдахаас өмнө шелл задалж өгдөг тусгай хэв загвар юм.

Одоогийн хавтас доторх бүх `.py` файлыг (рекурсив биш) устгая гэж бодъё. Өмнөх лекцээр сурсан зүйлээрээ бол ингэж хийж болно:

```shell
for file in $(ls | grep -P '\.py$'); do
    rm "$file"
done
```

Гэтэл үүнийг зүгээр `rm *.py` гэж бичихэд л болно!

Терминал дээр `rm *.py` гэж бичихэд шелл `/bin/rm`-ийг `['*.py']` аргументтай ажиллуулдаггүй. Харин одоогийн хавтас дотроос `*.py` хэв загварт таарах файлуудыг хайдаг; энд `*` нь дурын тэмдэгтүүдээс бүрдсэн тэг буюу түүнээс олон тэмдэгтийг илэрхийлнэ. Тэгэхээр хавтас дотор `main.py`, `utils.py` байвал `rm` програм `['main.py', 'utils.py']` гэсэн аргументууд авна.

Хамгийн их хэрэглэгддэг glob-ууд бол орлуулагч тэмдэгт (wildcard) `*` (дурын тэмдэгтийн тэг буюу түүнээс олон), `?` (яг нэг дурын тэмдэгт), мөн буржгар хаалт `{}`. Буржгар хаалт таслалаар тусгаарласан хэв загваруудын жагсаалтыг олон аргумент болгон задалдаг.

Glob-ыг жишээн дээр ойлгох нь хамгийн амар:

```shell
touch folder/{a,b,c}.py
# Will expand to
touch folder/a.py folder/b.py folder/c.py

convert image.{png,jpg}
# Will expand to
convert image.png image.jpg

cp /path/to/project/{setup,build,deploy}.sh /newpath
# Will expand to
cp /path/to/project/setup.sh /path/to/project/build.sh /path/to/project/deploy.sh /newpath

# Globbing techniques can also be combined
mv *{.py,.sh} folder
# Will move all *.py and *.sh files
```

> Зарим шелл (жишээ нь zsh) дэд хавтсуудыг хамардаг `**` гэх мэт илүү нарийн glob-уудыг дэмждэг. Тэгэхээр `rm **/*.py` бүх `.py` файлыг рекурсив байдлаар устгана.

## Урсгал

Ингэж хэд хэдэн програмыг дараалуулан ажиллуулахад:

```shell
cat myfile | grep -P '\d+' | uniq -c
```

`grep` програм `cat`, `uniq` хоёулантай нь харилцаж байгааг харж болно.

Энд анхаарах чухал зүйл бол гурван програм бүгд нэгэн зэрэг ажиллаж байгаа явдал. Өөрөөр хэлбэл, шелл эхлээд cat, дараа нь grep, дараа нь uniq-ийг дуудаж байгаа юм биш. Гурвуулаа зэрэг эхэлж, шелл cat-ын гаралтыг grep-ийн оролттой, grep-ийн гаралтыг uniq-ийн оролттой холбодог. Пайп (`|`) хэрэглэхэд шелл нэг програмаас нөгөө рүү урсах өгөгдлийн урсгал дээр ажилладаг.

Пайпаар холбосон бүх команд шууд эхэлдгийг ингэж шалгаж болно:

```console
$ (sleep 15 && cat numbers.txt) | grep -P '^\d$' | sort | uniq  &
[1] 12345
$ ps | grep -P '(sleep|cat|grep|sort|uniq)'
  32930 pts/1    00:00:00 sleep
  32931 pts/1    00:00:00 grep
  32932 pts/1    00:00:00 sort
  32933 pts/1    00:00:00 uniq
  32948 pts/1    00:00:00 grep
```

`cat`-аас бусад бүх процесс шууд ажиллаж байгааг харж байна. Шелл аль нэг нь дуусахыг хүлээлгүй бүх процессыг эхлүүлээд урсгалуудыг нь холбочихдог. `cat` зөвхөн sleep дууссаны дараа эхэлнэ, тэгээд `cat`-ын гаралт grep рүү урсаж, гэх мэтээр үргэлжилнэ.

Програм бүр стандарт оролт буюу `stdin` гэдэг оролтын урсгалтай. Пайпаар холбоход `stdin` автоматаар холбогддог. Скрипт дотор олон програм файлын нэрийн оронд `-`-г "`stdin`-ээс унш" гэсэн утгаар хүлээн авдаг:

```shell
# These are equivalent when data comes from a pipe
echo "hello" | grep "hello"
echo "hello" | grep "hello" -
```

Үүнтэй адил програм бүр стандарт гаралт буюу `stdout`, стандарт алдаа буюу `stderr` гэсэн хоёр гаралтын урсгалтай. `stdout` бол хамгийн их хэрэглэгддэг нь, дараагийн команд руу пайпаар дамждаг гаралт. `stderr` бол програмын анхааруулга болон бусад алдааны мессежийг хэвлэх тусдаа урсгал; ингэснээр тэдгээр нь дараагийн команд руу орж боловсруулагдахгүй.

```console
$ ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
$ ls /nonexistent | grep "pattern"
ls: cannot access '/nonexistent': No such file or directory
# The error message still appears because stderr is not piped
$ ls /nonexistent 2>/dev/null
# No output - stderr was redirected to /dev/null
```

Шелл эдгээр урсгалыг чиглүүлэх (redirect) синтакстай. Хэдэн жишээ:

```shell
# Redirect stdout to a file (overwrite)
echo "hello" > output.txt

# Redirect stdout to a file (append)
echo "world" >> output.txt

# Redirect stderr to a file
ls foobar 2> errors.txt

# Redirect both stdout and stderr to the same file
ls foobar &> all_output.txt

# Redirect stdin from a file
grep "pattern" < input.txt

# Discard output by redirecting to /dev/null
cmd > /dev/null 2>&1
```

Unix философийг сайн харуулдаг бас нэг хүчтэй хэрэгсэл бол бүдэг хайлт (fuzzy search) хийдэг [`fzf`](https://github.com/junegunn/fzf). `stdin`-ээс мөрүүд уншаад тэднийг шүүж, сонгох интерактив интерфейс гаргаж өгдөг:

```console
$ ls | fzf
$ cat ~/.bash_history | fzf
```

`fzf`-ийг шеллийн олон үйлдэлтэй холбож ашиглаж болно. Шеллээ тохируулах хэсэгт хэрэглээг нь дэлгэрэнгүй үзнэ.

## Орчны хувьсагч

Bash-д хувьсагч оноохдоо `foo=bar` гэж бичээд, утгыг нь `$foo`-гоор авна. `foo = bar` гэж бичвэл буруу: шелл үүнийг `foo` програмыг `['=', 'bar']` аргументтай дуудаж байна гэж ойлгоно. Шелл скриптэд хоосон зай аргументуудыг салгах үүрэгтэй. Энэ нь эхэндээ толгой эргүүлж, алдааг нь олоход хэцүү байж магадгүй тул анхаараарай.

Шеллийн хувьсагчид төрөлгүй, бүгд тэмдэгт мөр. Шелл дээр тэмдэгт мөр бичихэд дан хашилт, давхар хашилт хоёр өөр ажилладгийг санаарай. `'`-аар хашсан мөр яг бичсэнээрээ үлддэг: хувьсагч задлахгүй, командын орлуулга хийхгүй, тусгай тэмдэгт боловсруулахгүй. Харин `"`-аар хашсан мөр эдгээрийг бүгдийг нь хийнэ.

```shell
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```

Командын гаралтыг хувьсагчид хадгалахын тулд *командын орлуулга* (command substitution) хэрэглэнэ. Жишээ нь:

```shell
files=$(ls)
echo "$files" | grep README
echo "$files" | grep ".py"
```

`ls`-ийн гаралт (тодруулбал stdout) `$files` хувьсагчид хадгалагдаж, дараа нь хэрэглэж болно. `$files` дотор `ls`-ийн гаралтын шинэ мөрийн тэмдэгтүүд хэвээр үлддэг тул `grep` шиг програмууд мөр бүр дээр тусад нь ажиллаж чадна.

Үүнтэй төстэй ч тийм ч олон хүн мэддэггүй боломж бол *процессын орлуулга* (process substitution). `<( CMD )` нь `CMD`-г ажиллуулж, гаралтыг нь түр файлд хадгалаад `<()`-ийн оронд тэр файлын нэрийг тавьдаг. Програм утгаа stdin-ээр биш файлаар авахыг хүлээж байгаа үед их хэрэг болно. Жишээ нь `diff <(ls src) <(ls docs)` нь `src`, `docs` хавтсуудын файлуудын ялгааг харуулна.

Шеллээс өөр програм ажиллуулах болгонд тэр програмд *орчны хувьсагч* гэдэг хувьсагчдын багц дамждаг. Одоогийн орчны хувьсагчдыг шелл дээр `printenv` ажиллуулж харж болно. Орчны хувьсагчийг зөвхөн нэг програмд тусгайлан дамжуулах бол командын урд нь оноолтоо бичнэ:

> Орчны хувьсагчийг тогтсон дүрмээр ТОМ ҮСГЭЭР бичдэг (жишээ нь `HOME`, `PATH`, `DEBUG`). Техникийн хувьд заавал биш, гэхдээ ихэвчлэн жижиг үсгээр бичдэг энгийн хувьсагчдаас ялгахад тусалдаг.

```shell
TZ=Asia/Tokyo date  # prints the current time in Tokyo
echo $TZ  # this will be empty, since TZ was only set for the child command
```

Эсвэл одоогийн орчныг өөрчилдөг `export` командыг хэрэглэж болно. Ингэвэл үүнээс хойш эхлүүлсэн бүх дэд процесс тэр хувьсагчийг өвлөж авна:

```shell
export DEBUG=1
# All programs from this point onwards will have DEBUG=1 in their environment
bash -c 'echo $DEBUG'
# prints 1
```

Хувьсагчийг устгах бол шеллийн `unset` командыг хэрэглэнэ, жишээ нь `unset DEBUG`.

> Орчны хувьсагч бол шеллийн бас нэг тогтсон дүрэм. Олон програмын үйлдлийг шууд бусаар өөрчлөхөд хэрэглэгддэг. Жишээ нь шелл одоогийн хэрэглэгчийн home хавтасны замыг `$HOME` орчны хувьсагчид хадгалдаг. Тэгэхээр програмууд `--home /home/alice` гэж тусад нь өгүүлэх шаардлагагүй, `$HOME`-оос л уншчихна. Бас нэг жишээ нь `$TZ`: олон програм огноо, цагийг аль цагийн бүсээр харуулахаа үүгээр шийддэг.

## Гаралтын код

Өмнө үзсэнчлэн шелл програмын гол гаралт нь stdout/stderr урсгалууд болон файлын систем дээрх өөрчлөлтүүдээр дамжина.

Анхдагч байдлаар шелл скрипт 0 гаралтын код буцаадаг. Тогтсон дүрмээр 0 бол бүх зүйл амжилттай болсон, 0-ээс өөр бол ямар нэг алдаа гарсан гэсэн үг. 0-ээс өөр код буцаах бол шеллийн `exit NUM` командыг хэрэглэнэ. Хамгийн сүүлд ажилласан командын гаралтын кодыг тусгай `$?` хувьсагчаас авч болно.

Шеллд "БА", "ЭСВЭЛ" хийх `&&`, `||` логик оператор бий. Энгийн програмчлалын хэлнээс ялгаатай нь шеллд эдгээр нь програмуудын гаралтын код дээр ажилладаг. Хоёулаа [богино хэлхээний](https://en.wikipedia.org/wiki/Short-circuit_evaluation) (short-circuit) оператор: үр дүн аль хэдийн тодорхой болсон бол үлдсэн хэсгийг ажиллуулахгүй. Тэгэхээр өмнөх команд амжилттай (код 0) эсвэл амжилтгүй (0-ээс өөр) болсноос хамааруулж дараагийн командыг нөхцөлтэй ажиллуулахад хэрэглэж болно. Жишээ нь:

```shell
# echo will only run if grep succeeds (finds a match)
grep -q "pattern" file.txt && echo "Pattern found"

# echo will only run if grep fails (no match)
grep -q "pattern" file.txt || echo "Pattern not found"

# true is a shell program that always succeeds
true && echo "This will always print"

# and false is a shell program that always fails
false || echo "This will always print"
```

`if`, `while` ч мөн адил зарчмаар гаралтын кодоор шийдвэр гаргадаг:

```shell
# if uses the return code of the condition command (0 = true, nonzero = false)
if grep -q "pattern" file.txt; then
    echo "Found"
fi

# while loops continue as long as the command returns 0
while read line; do
    echo "$line"
done < file.txt
```

## Сигнал

Заримдаа ажиллаж байгаа програмыг таслах хэрэг гардаг, жишээ нь команд хэт удаан ажиллаж байвал. Хамгийн энгийн арга нь `Ctrl-C` дарах, тэгэхэд команд ихэвчлэн зогсдог. Гэхдээ цаана нь яг юу болдог вэ, яагаад заримдаа процесс зогсохгүй байдаг вэ?

```console
$ sleep 100
^C
$
```

> Энд `^C` нь терминал дээр `Ctrl-C` дарахад харагддаг тэмдэглэгээ.

Цаана нь ийм юм болсон:

1. Бид `Ctrl-C` дарсан
2. Шелл тэр тусгай товчны хослолыг таньсан
3. Шелл процесс `sleep` процесс руу `SIGINT` сигнал илгээсэн
4. Сигнал `sleep` процессын ажиллагааг тасалсан

Сигнал бол процессуудын харилцах тусгай механизм. Процесс сигнал хүлээн авахдаа ажиллагаагаа түр зогсоож, сигналыг боловсруулаад, сигналын агуулсан мэдээллээс хамаарч цаашид юу хийхээ өөрчилж магадгүй. Тиймээс сигналыг *програм хангамжийн тасалдал* (software interrupt) гэж нэрлэдэг.

Бидний жишээнд `Ctrl-C` дарахад шелл процесс руу `SIGINT` илгээсэн. Доор `SIGINT`-ийг барьж аваад үл тоомсорлодог, зогсохгүй жижиг Python програм байна. Энэ програмыг зогсоохын тулд `Ctrl-\` дарж `SIGQUIT` сигнал илгээх хэрэгтэй.

```python
#!/usr/bin/env python
import signal, time

def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
```

Энэ програм руу `SIGINT`-ийг хоёр удаа, дараа нь `SIGQUIT` илгээвэл ингэж харагдана. `^` нь терминал дээр `Ctrl` товчийг илэрхийлж байгааг анхаараарай.

```console
$ python sigint.py
24^C
I got a SIGINT, but I am not stopping
26^C
I got a SIGINT, but I am not stopping
30^\[1]    39913 quit       python sigint.py
```

`SIGINT`, `SIGQUIT` хоёр ихэвчлэн терминалаас ирэх хүсэлтэд хэрэглэгддэг бол процессыг эмх цэгцтэй (ажлаа цэгцлээд) зогсоохыг хүсэх ерөнхий сигнал нь `SIGTERM`. Үүнийг илгээхдээ [`kill`](https://www.man7.org/linux/man-pages/man1/kill.1.html) командыг `kill -TERM <PID>` гэж хэрэглэнэ.

Сигнал процессыг устгахаас өөр юм ч хийж чадна. Жишээ нь `SIGSTOP` процессыг түр зогсооно. Терминал дээр `Ctrl-Z` дарахад шелл `SIGTSTP` (Terminal Stop, өөрөөр хэлбэл `SIGSTOP`-ийн терминалын хувилбар) илгээдэг.

Түр зогсоосон ажлыг [`fg`](https://www.man7.org/linux/man-pages/man1/fg.1p.html) эсвэл [`bg`](https://man7.org/linux/man-pages/man1/bg.1p.html)-ээр урд талд (foreground) эсвэл ард (background) үргэлжлүүлж болно.

[`jobs`](https://www.man7.org/linux/man-pages/man1/jobs.1p.html) команд одоогийн терминалын сесстэй холбоотой дуусаагүй ажлуудыг жагсаана. Эдгээр ажлыг pid-ээр нь заагаад хандаж болно (pid олохдоо [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) хэрэглэж болно). Илүү хялбар нь `%` тэмдэгт ба ажлын дугаараар (`jobs`-ийн харуулдаг дугаар) хандах. Хамгийн сүүлд ард оруулсан ажилд `$!` тусгай параметрээр хандана.

Командын төгсгөлд `&` залгавал командыг шууд ард ажиллуулаад промптыг танд буцааж өгнө. Гэхдээ шеллийн stdout-ыг хэрэглэсээр байх тул гаралт нь дунд тань хэвлэгдээд саад болж магадгүй (чиглүүлэлтээр шийдэж болно). Мөн ажиллаж байгаа програмыг `Ctrl-Z` дарж түр зогсоогоод `bg` гэвэл ижил үр дүнд хүрнэ.

Ард ажиллаж буй процессууд терминалын тань дэд процесс хэвээр байх тул терминалаа хаавал тэд устна (терминал хаагдахад тэдэнд `SIGHUP` сигнал очдог). Үүнээс сэргийлэхийн тулд програмыг [`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html)-аар (`SIGHUP`-ыг үл тоомсорлодог бүрхүүл) ажиллуулах, эсвэл аль хэдийн ажиллаж байгаа процессыг `disown` командаар терминалаас салгаж болно. Эсвэл дараагийн хэсэгт үзэх терминал мультиплексор хэрэглэж болно.

Доорх жишээ эдгээр ойлголтуудыг харуулж байна.

```
$ sleep 1000
^Z
[1]  + 18653 suspended  sleep 1000

$ nohup sleep 2000 &
[2] 18745
appending output to nohup.out

$ jobs
[1]  + suspended  sleep 1000
[2]  - running    nohup sleep 2000

$ kill -SIGHUP %1
[1]  + 18653 hangup     sleep 1000

$ kill -SIGHUP %2   # nohup protects from SIGHUP

$ jobs
[2]  + running    nohup sleep 2000

$ kill %2
[2]  + 18745 terminated  nohup sleep 2000
```

`SIGKILL` бол онцгой сигнал: процесс үүнийг барьж авч чадахгүй тул процесс үргэлж шууд устна. Гэхдээ эзэнгүй үлдсэн дэд процессууд гэх мэт муу үр дагавартай байж болно.

Эдгээр болон бусад сигналын талаар [эндээс](https://en.wikipedia.org/wiki/Signal_%28IPC%29), эсвэл [`man signal`](https://www.man7.org/linux/man-pages/man7/signal.7.html), `kill -l` гэж бичиж дэлгэрэнгүй үзээрэй.

Шелл скрипт дотор сигнал ирэхэд команд ажиллуулахын тулд `trap` командыг хэрэглэж болно. Ажлын төгсгөлд цэвэрлэгээ хийхэд их хэрэгтэй:

```shell
#!/usr/bin/env bash
cleanup() {
    echo "Cleaning up temporary files..."
    rm -f /tmp/mytemp.*
}
trap cleanup EXIT  # Run cleanup when script exits
trap cleanup SIGINT SIGTERM  # Also on Ctrl-C or kill
```
{% comment %}
### Users, Files and Permissions

Lastly, another way programs have to indirectly communicate with each other is using files.
For a program to be able to correctly read/write/delete files and folders, the file permissions must allow the operation.

Listing a specific file will give the following output

```console
$ ls -l notes.txt
-rw-r--r--  1 alice  users  12693 Jan 11 23:05 notes.txt
```

Here `ls` is listing what is the owner of the file, user `alice`, and the group `users`. Then the `rw-r--r--` are a shorthand notation for the permissions.
In this case, the file `notes.txt` has read/write permissions for the user alice `rw-`, and only read permissions for the group and the rest of users in the file system.

```console
$ ./script.sh
# permission denied
$ chmod +x script.sh
$ ls -l script.sh
-rwxr-xr-x  1 alice  users  3125 Jan 11 23:07 script.sh
$ ./script.sh
```

For a script to be executable, the executable rights must be set, hence why we had to use the `chmod` (change mode) program.
`chmod` syntax, while intuitive, is not obvious when first encountered.
If you, like me, prefer to learn by example, this is a good usecase of the `tldr` tool (note that you need to install it first).

```console
❯ tldr chmod
  Change the access permissions of a file or directory.
  More information: <https://www.gnu.org/software/coreutils/chmod>.

  Give the [u]ser who owns a file the right to e[x]ecute it:

      chmod u+x path/to/file

  Give the [u]ser rights to [r]ead and [w]rite to a file/directory:

      chmod u+rw path/to/file_or_directory

  Give [a]ll users rights to [r]ead and e[x]ecute:

      chmod a+rx path/to/file
```

Run `tldr chmod` to see more examples, including recursive operations and group permissions.

> Your shell might show you something like `command not found: tldr`. That is because it is a more modern tool and it is not pre-installed in most systems. A good reference for how to install tools is the [https://command-not-found.com](https://command-not-found.com) website. It contains instructions for a huge collection of CLI tools for popular OS distributions.

Each program is run as a specific user in the system. We can use the `whoami` command to find our user name and `id -u` to find our UID (user id) which is the integer value that the OS associates with the user.

When running `sudo command`, the `command` is run as the root user which can bypass most permissions in the system.
Try running `sudo whoami` and `sudo id -u` to see how the output changes (you might be prompted for your password).
To change the owner of a file or folder, we use the `chown` command.

You can learn more about UNIX file permissions [here](https://en.wikipedia.org/wiki/File-system_permissions#Traditional_Unix_permissions)

So far we've focused on your local machine, but many of these skills become even more valuable when working with remote servers.

{% endcomment %}

# Алсын сервер

Өнөө үед програмистууд өдөр тутамдаа алсын сервертэй ажиллах нь улам түгээмэл болж байна. Үүнд хамгийн их хэрэглэгддэг хэрэгсэл бол SSH (Secure Shell): алсын сервертэй холбогдоод танил шелл интерфейсийг тань гаргаж өгдөг. Сервертэй ингэж холбогдоно:

```bash
ssh alice@server.mit.edu
```

Энд бид `server.mit.edu` сервер рүү `alice` хэрэглэгчээр нэвтрэхийг оролдож байна.

`ssh`-ийн хүмүүс тэр бүр анзаардаггүй нэг давуу тал нь командыг интерактив бусаар ажиллуулж болдогт байдаг. `ssh` командын stdin-ийг дамжуулж, stdout-ийг буцааж авах ажлыг зөв хийдэг тул бусад командтай амархан хослуулж болно:

```shell
# here ls runs in the remote, and wc runs locally
ssh alice@server ls | wc -l

# here both ls and wc run in the server
ssh alice@server 'ls | wc -l'

```

> Сүлжээ тасрах, зөөврийн компьютер унтах горимд орох, сүлжээ солигдох, сүлжээний саатал ихтэй үед ч найдвартай ажилладаг SSH-ийн орлогч [Mosh](https://mosh.org/)-г суулгаад үзээрэй.

Алсын сервер дээр команд ажиллуулахын тулд `ssh` таныг зөвшөөрөлтэй хэрэглэгч мөн гэдгийг баталгаажуулах ёстой. Үүнийг нууц үгээр эсвэл SSH түлхүүрээр хийж болно. Түлхүүрт суурилсан нэвтрэлт нийтийн түлхүүрийн криптограф (public-key cryptography) ашигладаг тул хувийн түлхүүрээ ил гаргалгүйгээр серверт өөрийгөө таниулна. Энэ арга илүү эвтэйхэн, бас аюулгүй тул боломжтой бол үүнийг ашигласан нь дээр. Хувийн түлхүүр (ихэвчлэн `~/.ssh/id_rsa`, сүүлийн үед `~/.ssh/id_ed25519`) нууц үгтэй адил эмзэг зүйл тул сайн хамгаалж, хэзээ ч бусадтай хуваалцаж болохгүй.

Түлхүүрийн хос үүсгэхдээ [`ssh-keygen`](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html) ажиллуулна.
```bash
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519
```

GitHub руу SSH түлхүүрээр push хийдэг бол [эдгээр](https://help.github.com/articles/connecting-to-github-with-ssh/) алхмыг аль хэдийн хийчихсэн, хүчинтэй түлхүүрийн хостой байгаа байх. Хувийн түлхүүр тань нууц үгтэй (passphrase) эсэхийг шалгаж, баталгаажуулах бол `ssh-keygen -y -f /path/to/key` ажиллуулна.

Серверийн талд `ssh` хэнийг нэвтрүүлэхээ `.ssh/authorized_keys` файлаас хардаг. Нийтийн түлхүүрээ сервер рүү хуулахдаа:

```bash
cat .ssh/id_ed25519.pub | ssh alice@remote 'cat >> ~/.ssh/authorized_keys'

# or more simply (if ssh-copy-id is available)

ssh-copy-id -i .ssh/id_ed25519 alice@remote
```

Команд ажиллуулахаас гадна ssh-ийн нээсэн холболтоор файлаа сервер рүү, эсвэл серверээс өөрийн машин руу найдвартай зөөж болно. [`scp`](https://www.man7.org/linux/man-pages/man1/scp.1.html) бол уламжлалт хэрэгсэл, синтакс нь `scp path/to/local_file remote_host:path/to/remote_file`. [`rsync`](https://www.man7.org/linux/man-pages/man1/rsync.1.html) нь хоёр талд аль файл аль хэдийн ижил байгааг таньж, дахин хуулахгүй гэдгээрээ `scp`-ээс давуу. Мөн симболик холбоос (symlink), эрхийн тохиргоог зөв шилжүүлэх, тасарсан хуулалтыг үргэлжлүүлэх `--partial` флаг гэх мэт нэмэлт боломжтой. `rsync`-ийн синтакс `scp`-тэй төстэй.

SSH-ийн клиент талын тохиргоо `~/.ssh/config` файлд байдаг. Тэнд серверүүд болон тэдгээрийн анхдагч тохиргоог зарлаж болно. Энэ тохиргоог зөвхөн `ssh` биш, `scp`, `rsync`, `mosh` зэрэг бусад програм ч уншдаг.

```bash
Host vm
    User alice
    HostName 172.16.174.141
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

# Configs can also take wildcards
Host *.mit.edu
    User alice
```

# Терминал мультиплексор

Командын мөрийн интерфейс хэрэглэхдээ олон юмыг зэрэг ажиллуулах хэрэг их гардаг. Жишээ нь засварлагч, програм хоёроо зэрэгцүүлээд харах. Олон терминалын цонх нээгээд ч болно, гэхдээ терминал мультиплексор (terminal multiplexer) хэрэглэх нь хамаагүй эвтэйхэн.

[`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) шиг терминал мультиплексор терминалын цонхоо хэсэг (pane), таб (window) болгон хувааж, олон шелл сесстэй үр дүнтэй ажиллах боломж олгодог. Мөн одоогийн терминалын сессээсээ салаад (detach) дараа нь буцаж холбогдож (attach) болно. Тиймээс алсын сервер дээр ажиллахад их тохиромжтой: `nohup` гэх мэт аргууд хэрэггүй болно.

Өнөөдөр хамгийн алдартай терминал мультиплексор бол [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html). `tmux` маш их тохируулагддаг бөгөөд товчны хослолоор олон таб, хэсэг үүсгэж, хооронд нь хурдан шилжиж болно.

`tmux` хэрэглэхийн тулд товчны хослолуудыг нь мэдэх хэрэгтэй. Бүгд `<C-b> x` хэлбэртэй: (1) `Ctrl+b` дарна, (2) `Ctrl+b`-ээ суллана, (3) `x` дарна. `tmux` дараах шатлалтай объектуудтай:
- **Сесс (session)** - нэг буюу хэд хэдэн цонхтой бие даасан ажлын талбар.
    + `tmux` шинэ сесс эхлүүлнэ.
    + `tmux new -s NAME` нэртэй сесс эхлүүлнэ.
    + `tmux ls` одоо байгаа сессүүдийг жагсаана.
    + `tmux` дотроос `<C-b> d` дарвал одоогийн сессээс салж гарна.
    + `tmux a` хамгийн сүүлийн сесс рүү буцаж холбогдоно. Аль рүү нь холбогдохоо `-t` флагаар зааж болно.

- **Цонх (window)** - засварлагч эсвэл хөтчийн таб шиг, нэг сесс доторх тусдаа хуудсууд.
    + `<C-b> c` шинэ цонх үүсгэнэ. Хаахдаа доторх шеллээ `<C-d>` дараад хаахад л болно.
    + `<C-b> N` *N* дугаартай цонх руу очно. Цонхнууд дугаарлагдсан байдгийг анхаараарай.
    + `<C-b> p` өмнөх цонх руу очно.
    + `<C-b> n` дараагийн цонх руу очно.
    + `<C-b> ,` одоогийн цонхны нэрийг өөрчилнө.
    + `<C-b> w` бүх цонхыг жагсаана.

- **Хэсэг (pane)** - Vim-ийн хуваалт шиг, нэг дэлгэц дээр олон шелл зэрэг харуулна.
    + `<C-b> "` одоогийн хэсгийг хэвтээ шугамаар хуваана.
    + `<C-b> %` одоогийн хэсгийг босоо шугамаар хуваана.
    + `<C-b> <direction>` заасан *direction* (чиглэл) дахь хэсэг рүү очно. Чиглэл гэдэг нь сумтай товчнууд.
    + `<C-b> z` одоогийн хэсгийг бүтэн дэлгэцээр томруулах / буцааж жижигрүүлэх.
    + `<C-b> [` гүйлгэж харах горимд орно. `<space>` дарж сонголт эхлүүлээд, `<enter>` дарж сонгосноо хуулна.
    + `<C-b> <space>` хэсгүүдийн байрлалыг ээлжлэн солино.

> tmux-ийн талаар илүү ихийг мэдэхийг хүсвэл [энэ](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) богино заавар, [энэ](https://linuxcommand.org/lc3_adv_termmux.php) дэлгэрэнгүй тайлбарыг уншаарай.

tmux, SSH хоёрыг эзэмшчихвэл аль ч машин дээр орчноо гэр шигээ эвтэйхэн болгомоор санагдана. Энд шеллээ тохируулах хэрэг гарна.

# Шеллээ өөртөө тааруулах

Олон командын мөрийн програм *dotfiles* гэдэг энгийн текст файлуудаар тохируулагддаг. Нэр нь `.`-ээр эхэлдэг тул (`~/.vimrc` гэх мэт) `ls` анхдагч байдлаараа эдгээрийг харуулдаггүй, нуугдмал байдаг.

> Dotfiles бол шеллийн бас нэг тогтсон дүрэм. Урд талын цэг нь жагсаалтад харагдахгүй "нуух" зорилготой.

Шелл өөрөө ийм файлаар тохируулагддаг програмын нэг жишээ. Асахдаа шелл тохиргоогоо ачаалахын тулд олон файл уншдаг. Ямар шелл вэ, нэвтрэх (login) сесс үү, интерактив сесс үү гэдгээс хамаараад энэ үйл явц нэлээд ярвигтай байж болно. Энэ сэдвээр маш сайн материал [энд](https://web.archive.org/web/20260329133158/https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html) байна.

`bash`-ийн хувьд ихэнх системд `.bashrc` эсвэл `.bash_profile`-ыг засахад л болно. Dotfiles-аар тохируулж болох бусад хэрэгслүүд:

- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` болон `~/.vim` хавтас
- `ssh` - `~/.ssh/config`
- `tmux` - `~/.tmux.conf`

Хамгийн түгээмэл тохиргооны өөрчлөлт бол шеллийн програм хайх шинэ байршил нэмэх. Програм суулгахдаа энэ хэв загвартай олонтаа таарна:

```shell
export PATH="$PATH:path/to/append"
```

Энд бид шеллд `$PATH` хувьсагчийг одоогийн утга дээр нь шинэ зам нэмж оноо, бүх дэд процесс энэ шинэ `PATH`-ыг өвлөж ав гэж хэлж байна. Ингэснээр дэд процессууд `path/to/append` доторх програмуудыг олж чадна.

Шеллээ тохируулна гэдэг ихэвчлэн шинэ командын мөрийн хэрэгсэл суулгана гэсэн үг. Багц менежерүүд (package manager) үүнийг амар болгодог: програм татах, суулгах, шинэчлэх ажлыг хариуцна. Үйлдлийн систем бүр өөрийн багц менежертэй: macOS дээр [Homebrew](https://brew.sh/), Ubuntu/Debian дээр `apt`, Fedora дээр `dnf`, Arch дээр `pacman`. Багц менежерүүдийн талаар [Кодыг багцлах ба хүргэх](/2026/shipping-code/) лекцээр дэлгэрэнгүй үзнэ.

macOS дээр Homebrew-ээр хоёр хэрэгтэй програм суулгах нь:

```shell
# ripgrep: a faster grep with better defaults
brew install ripgrep

# fd: a faster, user-friendly find
brew install fd
```

Эдгээрийг суулгачихвал `grep`-ийн оронд `rg`, `find`-ын оронд `fd` хэрэглэж болно.

> **`curl | bash`-ийн талаар анхааруулга**: `curl -fsSL https://example.com/install.sh | bash` шиг суулгах зааврыг олонтаа харна. Энэ хэлбэр скриптийг татаад шууд ажиллуулдаг: эвтэйхэн ч эрсдэлтэй, учир нь шалгаагүй кодоо ажиллуулж байгаа юм. Илүү аюулгүй арга нь эхлээд татаад, уншиж шалгаад, дараа нь ажиллуулах:
> ```shell
> curl -fsSL https://example.com/install.sh -o install.sh
> less install.sh  # review the script
> bash install.sh
> ```
> Зарим суулгагч арай аюулгүй хувилбар хэрэглэдэг: `/bin/bash -c "$(curl -fsSL https://url)"`. Ядаж л скриптийг таны одоогийн шелл биш bash тайлбарлаж ажиллуулна гэдгийг баталгаажуулдаг.

Суугаагүй команд ажиллуулах гэвэл шелл `command not found` гэж гаргана. [command-not-found.com](https://command-not-found.com) сайт дээр дурын командыг хайгаад, өөр өөр багц менежер, дистрибутив дээр яаж суулгахыг нь харж болно.

Бас нэг хэрэгтэй хэрэгсэл бол [`tldr`](https://tldr.sh/): хялбаршуулсан, жишээнд төвлөрсөн man хуудас гаргаж өгдөг. Урт баримт бичиг уншихын оронд түгээмэл хэрэглээг нь шууд харна:

```console
$ tldr fd
  An alternative to find.
  Aims to be faster and easier to use than find.

  Recursively find files matching a pattern in the current directory:
      fd "pattern"

  Find files that begin with "foo":
      fd "^foo"

  Find files with a specific extension:
      fd --extension txt
```

Заримдаа бүтэн шинэ програм хэрэггүй, байгаа командыг тодорхой флагуудтай нь товчилж дуудах л хэрэгтэй болдог. Энд alias хэрэг болно.

Шеллийн `alias` командаар өөрийн товчлол үүсгэж болно. Шеллийн alias гэдэг нь өөр командын товч нэр бөгөөд шелл командыг ажиллуулахаас өмнө автоматаар орлуулдаг. Жишээ нь bash-д alias ийм бүтэцтэй:

```bash
alias alias_name="command_to_alias arg1 arg2"
```

> `=` тэмдгийн хоёр талд хоосон зай байхгүйг анхаараарай: [`alias`](https://www.man7.org/linux/man-pages/man1/alias.1p.html) нь ганц аргумент авдаг шеллийн команд учраас тэр.

Alias олон эвтэйхэн боломжтой:

```bash
# Түгээмэл флагуудад товчлол үүсгэх
alias ll="ls -lh"

# Байнга бичдэг урт командуудыг хурдан бичих
alias gs="git status"
alias gc="git commit"

# Буруу бичихээс сэргийлэх
alias sl=ls

# Байгаа командуудыг илүү зөв анхдагч тохиргоотой болгох
alias mv="mv -i"           # -i нь файлыг дарж бичихээс өмнө асууна
alias mkdir="mkdir -p"     # -p нь эх хавтас байхгүй бол үүсгэнэ
alias df="df -h"           # -h нь хэмжээг хүнд уншигдахаар харуулна

# Alias-уудыг хооронд нь хослуулж болно
alias la="ls -A"
alias lla="la -l"

# Alias-ыг тойрч жинхэнэ командыг ажиллуулах бол урд нь \ тавина
\ls
# Эсвэл unalias-аар alias-ыг бүрмөсөн устгана
unalias la

# Alias-ын тодорхойлолтыг харах бол alias-аар дуудна
alias ll
# ll='ls -lh' гэж хэвлэнэ
```

Alias-д хязгаарлалт бий: командын дунд аргумент авч чаддаггүй. Илүү нарийн зүйл хэрэгтэй бол шеллийн функц хэрэглээрэй.

Ихэнх шелл `Ctrl-R`-ээр түүхээсээ ухрааж хайх боломжтой. `Ctrl-R` дараад бичиж эхэлбэл өмнөх командуудаас хайна. Өмнө `fzf`-ийг танилцуулсан; fzf-ийг шеллтэйгээ холбож тохируулчихвал `Ctrl-R` бүх түүхээр интерактив бүдэг хайлт хийдэг болох бөгөөд анхныхаасаа хамаагүй хүчтэй болно.

Dotfiles-аа яаж зохион байгуулах вэ? Тусдаа хавтаст, хувилбарын хяналтад
хадгалаад, скрипт ашиглан байх ёстой газар нь **симболик холбоос** (symlink)
хийх хэрэгтэй. Давуу тал нь:

- **Суулгахад амар**: шинэ машин руу нэвтэрмэгц өөрийн тохиргоогоо нэг минутанд
хэрэгжүүлнэ.
- **Хаана ч адилхан**: хэрэгслүүд тань хаа сайгүй адилхан ажиллана.
- **Тохиргоо нэгтгэх**: dotfiles-аа хаанаас ч шинэчлээд бүх машин дээрээ ижил
байлгана.
- **Өөрчлөлтөө хянах**: dotfiles-аа магадгүй програмистын бүх карьерынхаа турш
арчлах байх, урт хугацааны төсөлд өөрчлөлтийн түүх байх нь сайхан.

Dotfiles-даа юу хийх вэ? Хэрэгслүүдийнхээ тохиргооны талаар онлайн баримт бичиг
эсвэл [man хуудас](https://en.wikipedia.org/wiki/Man_page) уншаад мэдэж болно. Бас
тухайн програмын талаарх блог нийтлэлүүдийг интернэтээс хайж болно, тэнд зохиогчид
дуртай тохиргоогоо хуваалцдаг. Бас нэг арга нь бусдын dotfiles-ыг ухаж үзэх:
GitHub дээр тоо томшгүй олон [dotfiles
репо](https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories)
бий --- хамгийн алдартайг нь [эндээс](https://github.com/mathiasbynens/dotfiles)
үзээрэй (гэхдээ тохиргоог сохроор хуулахгүй байхыг зөвлөж байна).
[Энэ](https://dotfiles.github.io/) бас энэ сэдвээр сайн материал.

Хичээлийн бүх багш нарын dotfiles GitHub дээр нээлттэй: [Anish](https://github.com/anishathalye/dotfiles),
[Jon](https://github.com/jonhoo/configs),
[Jose](https://github.com/jjgo/dotfiles).

**Фреймворк, плагин**-ууд ч шеллийг тань сайжруулж чадна. Алдартай ерөнхий фреймворкууд нь [prezto](https://github.com/sorin-ionescu/prezto), [oh-my-zsh](https://ohmyz.sh/), мөн тодорхой нэг боломжид төвлөрсөн жижиг плагинууд:

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) - бичиж байхад зөв/буруу командыг өнгөөр ялгана
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) - бичиж байхад түүхээсээ команд санал болгоно
- [zsh-completions](https://github.com/zsh-users/zsh-completions) - автомат нөхөлтийн нэмэлт тодорхойлолтууд
- [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search) - fish шиг түүхээс хайх
- [powerlevel10k](https://github.com/romkatv/powerlevel10k) - хурдан, тохируулж болдог промптын загвар

[fish](https://fishshell.com/) шиг шелл эдгээр боломжийн ихэнхийг анхнаасаа агуулдаг.

> Эдгээр боломжийг авахын тулд oh-my-zsh шиг асар том фреймворк заавал хэрэггүй. Плагинуудыг тус тусад нь суулгах нь ихэвчлэн хурдан, бас илүү хяналт өгдөг. Том фреймворкууд шелл асах хугацааг мэдэгдэхүйц удаашруулдаг тул бодитоор хэрэглэдэг зүйлээ л суулгаарай.


# Шелл дэх AI

AI хэрэгслүүдийг шеллд нэвтрүүлэх олон арга бий. Өөр өөр түвшний хэдэн жишээ:

**Команд үүсгэх**: [`simonw/llm`](https://github.com/simonw/llm) шиг хэрэгслүүд энгийн хэлээр бичсэн тайлбараас шелл команд гаргаж өгдөг:

```console
$ llm cmd "find all python files modified in the last week"
find . -name "*.py" -mtime -7
```

**Пайпад оруулах**: LLM-ийг шеллийн пайпад оруулж өгөгдөл боловсруулж, хувиргаж болно. Ялангуяа формат нь тогтворгүй, regex бичих гэвэл толгой өвдөм өгөгдлөөс мэдээлэл сугалахад их хэрэгтэй:

```console
$ cat users.txt
Contact: john.doe@example.com
User 'alice_smith' logged in at 3pm
Posted by: @bob_jones on Twitter
Author: Jane Doe (jdoe)
Message from mike_wilson yesterday
Submitted by user: sarah.connor
$ INSTRUCTIONS="Extract just the username from each line, one per line, nothing else"
$ llm "$INSTRUCTIONS" < users.txt
john.doe
alice_smith
bob_jones
jdoe
mike_wilson
sarah.connor
```

Хувьсагч дотор хоосон зай байгаа тул `"$INSTRUCTIONS"`-ыг хашилттай бичсэн, файлын агуулгыг stdin рүү чиглүүлэхийн тулд `< users.txt` хэрэглэснийг анхаараарай.

**AI шелл**: [Claude Code](https://docs.anthropic.com/en/docs/claude-code) шиг хэрэгслүүд шеллийн дээр ажилладаг "мета-shell" болж ажилладаг: энгийн хэлээр өгсөн даалгаврыг шеллийн үйлдэл, файлын засвар, олон алхамтай нарийн ажил болгон хувиргана.

# Терминал эмулятор

Шеллээ тохируулахаас гадна ямар **терминал эмулятор** хэрэглэх, түүнийгээ яаж тохируулахад бага зэрэг цаг гаргах нь зүйтэй. Терминал эмулятор бол таны шелл ажилладаг текстэн интерфейсийг гаргаж өгдөг GUI програм. Сонголт маш олон бий.

Терминал дээрээ хэдэн зуу, магадгүй хэдэн мянган цаг өнгөрүүлэх тул тохиргоонд нь анхаарах нь үр өгөөжөө өгнө. Терминалаас өөрчилж болох зарим зүйл:

- Фонт
- Өнгөний схем
- Товчны хослол
- Таб/хэсэг (pane) дэмжлэг
- Гүйлгэж харах түүхийн тохиргоо
- Хурд, гүйцэтгэл (энд [Alacritty](https://github.com/alacritty/alacritty), [Ghostty](https://ghostty.org/) шиг GPU-ээр хурдасгадаг шинэ терминалууд давуутай).

# Дасгал ажил

## Аргумент ба glob

1. `cmd --flag -- --notaflag` гэх мэт командуудыг харах үе гарна. `--` нь програмд "эндээс хойш флаг задлахаа боль" гэдэг тусгай аргумент. `--`-ийн дараах бүх зүйлийг байрлалын аргумент (positional argument) гэж үзнэ. Энэ яагаад хэрэгтэй вэ? `touch -- -myfile` ажиллуулаад, дараа нь `--` хэрэглэлгүйгээр устгаж үзээрэй.

1. [`man ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html)-ийг уншаад файлуудыг ингэж жагсаах `ls` команд бичээрэй:
    - Нуугдмал файлуудыг оролцуулаад бүх файлыг харуулна
    - Хэмжээг хүнд уншигдахаар харуулна (жишээ нь 454279954-ийн оронд 454M)
    - Файлуудыг хамгийн сүүлд өөрчлөгдсөнөөр нь эрэмбэлнэ
    - Гаралт нь өнгөтэй байна

    Жишээ гаралт иймэрхүү харагдана:

    ```
    -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
    drwxr-xr-x   5 user group  160 Jan 14 09:53 .
    -rw-r--r--   1 user group  514 Jan 14 06:42 bar
    -rw-r--r--   1 user group 106M Jan 13 12:12 foo
    drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```

{% comment %}
ls -lath --color=auto
{% endcomment %}

1. Процессын орлуулга `<(command)` нь командын гаралтыг файл шиг хэрэглэх боломж өгдөг. Процессын орлуулга ашиглан `diff`-ээр `printenv`, `export` хоёрын гаралтыг харьцуулаарай. Яагаад ялгаатай байна вэ? (Сэжүүр: `diff <(printenv | sort) <(export | sort)` ажиллуулаад үзээрэй).

## Орчны хувьсагч

1. `marco`, `polo` гэсэн bash функц бичээрэй: `marco` ажиллуулах бүрд одоогийн ажлын хавтас хадгалагдана, дараа нь хаана ч байсан `polo` ажиллуулахад `marco` ажиллуулсан хавтас руу тань `cd` хийнэ. Алдаагаа олоход амар байлгах үүднээс кодоо `marco.sh` файлд бичээд `source marco.sh` гэж шеллдээ ачаалж болно.

{% comment %}
marco() {
    export MARCO=$(pwd)
}
polo() {
    cd "$MARCO"
}
{% endcomment %}

## Гаралтын код

1. Ховорхон алдаа заадаг нэг команд байна гэж бодъё. Алдааг нь олохын тулд гаралтыг нь барих хэрэгтэй ч, алдаа заах хүртэл гараараа дахин дахин ажиллуулахад цаг их зарна. Доорх скриптийг алдаа заах хүртэл ажиллуулж, стандарт гаралт, стандарт алдааны урсгалуудыг нь файлд хадгалаад, төгсгөлд нь бүгдийг хэвлэдэг bash скрипт бичээрэй. Алдаа заах хүртэл хэдэн удаа ажилласныг бас хэвлэж чадвал бонус оноо.

    ```bash
    #!/usr/bin/env bash

    n=$(( RANDOM % 100 ))

    if [[ n -eq 42 ]]; then
       echo "Something went wrong"
       >&2 echo "The error was using magic numbers"
       exit 1
    fi

    echo "Everything went according to plan"
    ```

{% comment %}
#!/usr/bin/env bash

count=0
until [[ "$?" -ne 0 ]];
do
    count=$((count+1))
    ./script.sh &> out.txt
done
echo "found error after $count runs"
cat out.txt
{% endcomment %}

## Сигнал ба ажлын удирдлага

1. Терминал дээр `sleep 10000` ажил эхлүүлээд, `Ctrl-Z`-ээр түр зогсоож, `bg`-ээр ард үргэлжлүүлээрэй. Одоо [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html)-ээр pid-ийг нь олоод, [`pkill`](https://man7.org/linux/man-pages/man1/pgrep.1.html)-ээр pid-ийг гараар бичилгүйгээр устгаарай. (Сэжүүр: `-lf` флагуудыг хэрэглээрэй).

1. Нэг процесс дуусахаас нааш өөр процесс эхлүүлэхийг хүсэхгүй байна гэж бодъё. Яаж хийх вэ? Энэ дасгалд хүлээх процесс маань үргэлж `sleep 60 &` байна. Нэг арга нь [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) команд хэрэглэх. sleep командыг эхлүүлээд, `ls`-ийг арын процесс дуустал хүлээлгээд үзээрэй.

    Гэхдээ өөр bash сессээс эхлүүлбэл энэ арга ажиллахгүй, учир нь `wait` зөвхөн дэд процесс дээр ажилладаг. Тэмдэглэлд дурдаагүй нэг зүйл: `kill` команд амжилттай бол гаралтын код 0, үгүй бол 0-ээс өөр буцаадаг. `kill -0` сигнал илгээдэггүй ч процесс байхгүй бол 0-ээс өөр код буцаана. Pid аваад тухайн процесс дуусах хүртэл хүлээдэг `pidwait` нэртэй bash функц бичээрэй. Процессорыг дэмий зарахгүйн тулд `sleep` хэрэглээрэй.

## Файл ба эрх

1. (Ахисан түвшин) Хавтас дотроос хамгийн сүүлд өөрчлөгдсөн файлыг рекурсив байдлаар олдог команд эсвэл скрипт бичээрэй. Ерөнхийлбөл, бүх файлыг сүүлд өөрчлөгдсөнөөр нь эрэмбэлж жагсааж чадах уу?

## Терминал мультиплексор

1. Энэ `tmux` [заавар](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)-ыг дагаад, дараа нь [эдгээр алхмаар](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/) үндсэн тохиргоо хийж сураарай.

## Alias ба dotfiles

1. `cd`-г буруу бичихэд зориулж `dc` гэсэн alias үүсгээрэй.

1. `history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10` ажиллуулаад хамгийн их хэрэглэдэг 10 командаа олж, тэдгээрт богино alias хийх талаар бодоорой. Анхаар: энэ Bash дээр ажиллана; ZSH хэрэглэдэг бол `history`-ын оронд `history 1` гэж бичээрэй.

1. Dotfiles-даа зориулж хавтас үүсгээд хувилбарын хяналтад (Git) оруулаарай.

1. Ядаж нэг програмын тохиргоог dotfiles-даа нэмээрэй (эхлээд `$PS1`-ээр шеллийн промптоо өөрчлөхөөс эхэлж болно).

1. Dotfiles-аа шинэ машин дээр хурдан (гар ажиллагаагүйгээр) суулгах арга гаргаарай. Файл бүр дээр `ln -s` дууддаг энгийн шелл скрипт ч байж болно, эсвэл [тусгай хэрэгсэл](https://dotfiles.github.io/utilities/) хэрэглэж болно.

1. Суулгах скриптээ шинэ виртуал машин дээр туршаад үзээрэй.

1. Одоо хэрэглэж байгаа бүх хэрэгслийнхээ тохиргоог dotfiles репо руугаа шилжүүлээрэй.

1. Dotfiles-аа GitHub дээр нийтлээрэй.

## Алсын сервер (SSH)

Эдгээр дасгалд зориулж Linux виртуал машин суулгаарай (эсвэл аль хэдийн байгаа нэгийг ашиглаж болно). Виртуал машиныг сайн мэдэхгүй бол суулгах талаар [энэ заавар](https://hibbard.eu/install-ubuntu-virtual-box/)-ыг үзээрэй.

1. `~/.ssh/` руу ороод SSH түлхүүрийн хос байгаа эсэхийг шалгаарай. Байхгүй бол `ssh-keygen -a 100 -t ed25519`-ээр үүсгээрэй. Түлхүүртээ нууц үг тавьж, `ssh-agent` хэрэглэхийг зөвлөж байна; дэлгэрэнгүйг [эндээс](https://www.ssh.com/ssh/agent) үзээрэй.

1. `.ssh/config`-ээ ингэж засаарай:

    ```bash
    Host vm
        User username_goes_here
        HostName ip_goes_here
        IdentityFile ~/.ssh/id_ed25519
        LocalForward 9999 localhost:8888
    ```

1. `ssh-copy-id vm`-ээр ssh-ийн нийтийн түлхүүрээ сервер рүү хуулаарай.

1. Виртуал машин дотроо `python -m http.server 8888` гэж вэб сервер асаагаарай. Өөрийн машин дээрээ хөтчөөр `http://localhost:9999` руу орж виртуал машины вэб серверт хандаарай.

1. `sudo vim /etc/ssh/sshd_config`-ээр SSH серверийн тохиргоогоо засаж, `PasswordAuthentication`-ы утгыг өөрчлөн нууц үгээр нэвтрэхийг хаагаарай. `PermitRootLogin`-ы утгыг өөрчлөөд root-оор нэвтрэхийг бас хаагаарай. `sudo service sshd restart`-аар `ssh` үйлчилгээгээ дахин эхлүүлээд дахин нэвтэрч үзээрэй.

1. (Сорилт) Виртуал машин дээрээ [`mosh`](https://mosh.org/) суулгаад холбогдоорой. Дараа нь сервер/виртуал машины сүлжээний адаптерыг салгаарай. mosh холболтоо сэргээж чадаж байна уу?

1. (Сорилт) `ssh`-ийн `-N`, `-f` флагууд юу хийдгийг судлаад ард нь порт дамжуулах (port forwarding) командыг олоорой.
