---
layout: lecture
title: "Командын мөрийн орчин"
description: >
  Командын мөрийн програмууд хэрхэн ажилладаг, үүний дотор оролт/гаралтын урсгалууд, орчны хувьсагчид болон SSH ашиглан алсын сервертэй холбогдох талаар суралцана.
thumbnail: /static/assets/thumbnails/2026/lec2.png
date: 2026-01-13
ready: true
video:
  aspect: 56.25
  id: ccBGsPedE9Q
---

Өмнөх лекцээр үзсэнчлэн shell бол зүгээр нэг програм ажиллуулагч биш. Ихэнх shell
өөрөө бүрэн хэмжээний scripting хэлтэй, дээр нь олон нийтлэг pattern, abstraction
агуулдаг. Гэхдээ Python, Ruby шиг ерөнхий зориулалтын хэлнүүдээс ялгаатай нь shell
script-ийн төвд програм ажиллуулах, тэдгээрийг хооронд нь эвтэйхэн холбох санаа
байдаг.

Shell script бичихэд *convention* их чухал. CLI програм shell орчинд эвтэйхэн
ажиллахын тулд хүмүүсийн хүлээдэг хэдэн нийтлэг дүрмийг дагадаг. Одоо CLI
програмууд яаж ажилладаг, яаж тохируулдаг, shell-тэй яаж “сайн иргэн” шиг
харилцдагийг үзье.

# CLI интерфейс

Ихэнх програмчлалын хэл дээр функц бичихэд ингэж харагддаг:

```
def add(x: int, y: int) -> int:
    return x + y
```

Энд оролт, гаралт нь шууд харагдаж байна. Shell script харин эхлээд харахад арай
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

Ийм script дотор юу болж байгааг ойлгохын тулд shell програмууд хоорондоо болон
shell орчинтой харилцахдаа ашигладаг хэдэн үндсэн ойлголтыг эхлээд авъя:

- Аргументууд (Arguments)
- Урсгалууд (Streams)
- Орчны хувьсагчид (Environment variables)
- Гаралтын кодууд (Return codes)
- Сигналууд (Signals)

## Аргументууд

Shell програм ажиллахдаа аргументын жагсаалт авдаг. Shell-ийн хувьд аргументууд
зүгээр л string; харин тэдгээрийг яаж тайлбарлах нь тухайн програмаас хамаарна.
Жишээ нь `ls -l folder/` гэж ажиллуулахад shell `/bin/ls`-ийг `['-l', 'folder/']`
гэсэн аргументуудтай дуудна.

Shell script дотор эдгээрийг тусгай syntax-аар авна. Эхний аргумент `$1`, хоёр дахь
нь `$2` гэх мэтээр `$9` хүртэл явна. Бүх аргументыг жагсаалт байдлаар авах бол
`$@`, аргументын тоо хэрэгтэй бол `$#` ашиглана. Ажиллаж байгаа script/program-ийн
нэр `$0` дээр байдаг.

Ихэнх програмын хувьд аргументууд нь *flags* болон ердийн тэмдэгт мөрүүдийн холимог байдаг. Flag-уудыг нэг зураас (`-`) эсвэл давхар зураас (`--`)-аар эхэлсэн байдлаар нь ялгаж болно. Flag нь ихэвчлэн сонголтоор ашиглагддаг бөгөөд програмын ажиллагааг өөрчлөх үүрэгтэй. Жишээлбэл, `ls -l` нь `ls` командын гаралтыг хэрхэн форматлахыг өөрчилдөг.

Та `--all` гэх мэт урт нэртэй давхар зураастай flag болон ихэвчлэн ганц үсгээс бүрдэх `-a` гэх мэт нэг зураастай flag-уудыг харах болно. Ижил тохиргоог хоёр хэлбэрээр зааж өгч болно: `ls -a` болон `ls --all` нь ижил утгатай. Нэг зураастай flag-уудыг ихэвчлэн нэгтгэж бичдэг тул `ls -l -a` болон `ls -la` нь мөн ижил утгатай. Flag-уудын дараалал ч гаралтын үр дүнд нөлөөлөхгүй: `ls -la` болон `ls -al` нь ижил үр дүн үзүүлнэ. Зарим flag маш түгээмэл ашиглагддаг бөгөөд та shell орчинтой илүү танилцах тусам тэдгээрийг шууд ашигладаг болно (жишээ нь, `--help`, `--verbose`, `--version`).

> Flags нь shell-ийн дүрмүүдийн (conventions) маш сайн жишээ юм. Shell хэл нь програмаа заавал `-` эсвэл `--` ашиглахыг шаарддаггүй. Та `myprogram +myoption myfile` гэсэн синтакстай програм бичиж болох ч энэ нь бусдыг төөрөгдүүлнэ, учир нь хүмүүс зураас ашигласан хэлбэрийг хүлээдэг.
> Практикт ихэнх програмчлалын хэл CLI flag задлан шинжлэх бэлэн сангуудыг (жишээ нь, Python дээрх `argparse`) санал болгодог.

CLI програмуудын өөр нэг нийтлэг дүрэм бол програмууд нь ижил төрлийн олон тооны аргумент хүлээн авдаг юм. Ийм байдлаар аргумент өгөхөд команд нь тэдгээрийн файл/folder бүр дээр ижил үйлдлийг гүйцэтгэдэг.

```shell
mkdir src
mkdir docs
# is equivalent to
mkdir src docs
```

Энэ хялбар синтакс нь эхлээд харахад шаардлагагүй мэт санагдаж болох ч *globbing*-той хамтран ашиглах үед маш сайн болж ирдэг. Globbing эсвэл globs нь shell програмыг дуудахаас өмнө өргөжүүлэн хувиргадаг тусгай хэв маяг юм.

Бид одоогийн folder доторх бүх `.py` файлуудыг рекурсив бус байдлаар устгахыг хүссэн гэж бодъё. Өмнөх лекцээр сурсан зүйлээр бол бид үүнийг ингэж хийж болно:

```shell
for file in $(ls | grep -P '\.py$'); do
    rm "$file"
done
```

Гэвч бид үүнийг ердөө `rm *.py` гэж орлуулж болно!

Бид терминал дээр `rm *.py` гэж бичих үед shell `/bin/rm` програмыг `['*.py']` гэсэн аргументтай ажиллуулдаггүй. Үүний оронд shell одоогийн folderнаас `*.py` загварт таарч буй файлуудыг хайдаг бөгөөд эндэх `*` нь ямар ч төрлийн тэмдэгтээс бүрдсэн хоосон буюу түүнээс олон тэмдэгт мөрийг илэрхийлнэ. Тиймээс хэрэв манай folder дотор `main.py` болон `utils.py` байгаа бол `rm` програм `['main.py', 'utils.py']` гэсэн аргументуудыг хүлээн авах болно.

Хамгийн түгээмэл хэрэглэгддэг globs нь орлуулах тэмдэгт `*` (юу ч байж болох хоосон эсвэл олон тэмдэгт), `?` (яг нэг дурын тэмдэгт) болон хаалт `{}` юм. Хаалт `{}` нь таслалаар тусгаарлагдсан загваруудын жагсаалтыг олон аргумент болгон задлан өргөжүүлдэг.

Практикт globs-ийг тодорхой жишээнүүдээр ойлгох нь илүү хялбар байдаг.

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

> Зарим shell-үүд (жишээ нь, zsh) рекурсив замыг багтаасан `**` зэрэг илүү нарийн globbing-ийг дэмждэг. Тиймээс `rm **/*.py` нь бүх `.py` файлуудыг рекурсив байдлаар устгах болно.

## Урсгалууд

Бид ингэж програмуудын дарааллыг ажиллуулах үед:

```shell
cat myfile | grep -P '\d+' | uniq -c
```

`grep` програм нь `cat` болон `uniq` програмуудын аль алинтай нь харилцаж байгааг харж болно.

Энд анхаарах чухал зүйл бол бүх гурван програм нэгэн зэрэг ажиллаж байгаа явдал юм. Өөрөөр хэлбэл, shell эхлээд cat-ыг ажиллуулаад, дараа нь grep-ийг, дараа нь uniq-ийг ажиллуулдаггүй. Үүний оронд бүх гурван процесс нэгэн зэрэг үүсч, shell нь cat-ын гаралтыг grep-ийн оролттой, grep-ийн гаралтыг uniq-ийн оролттой холбодог. Пайп (`|`) операторыг ашиглах үед shell нь дарааллын дагуу нэг програмаас дараагийн програм руу урсах өгөгдлийн урсгал дээр ажилладаг.

Pipeline доторх command-ууд бүгд шууд эхэлдгийг ингэж шалгаж болно:

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

Бид `cat`-аас бусад бүх процессууд шууд ажиллаж байгааг харж байна. Shell нь аль нэг процесс дуусахаас өмнө бүх процессуудыг үүсгэж, тэдгээрийн урсгалуудыг холбодог. `cat` нь зөвхөн sleep дууссаны дараа эхлэх бөгөөд `cat`-ын гаралт grep руу дамжих гэх мэтээр үргэлжилнэ.

Програм бүр standard input буюу `stdin` гэж нэрлэгддэг оролтын урсгалтай байдаг. Pipe хийх үед `stdin` автоматаар холбогддог. Скрипт дотор олон програмууд `stdin`-ээс уншихыг илэрхийлэхийн тулд файлын нэрний оронд `-` тэмдэгтийг хүлээн авдаг:

```shell
# These are equivalent when data comes from a pipe
echo "hello" | grep "hello"
echo "hello" | grep "hello" -
```

Үүнтэй адилаар програм бүр standard output буюу `stdout`, мөн standard error буюу `stderr` гэсэн хоёр гаралтын урсгалтай байдаг. `stdout` нь хамгийн түгээмэл хэрэглэгддэг урсгал бөгөөд програмын гаралтыг дараагийн command руу pipe хийхэд ашиглагддаг. `stderr` нь програмын анхааруулга болон бусад алдааны мэдээллүүдийг хэвлэхэд зориулагдсан урсгал бөгөөд ингэснээр уг гаралт дараагийн command руу дамжин боловсруулагдахаас сэргийлдэг.

```console
$ ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
$ ls /nonexistent | grep "pattern"
ls: cannot access '/nonexistent': No such file or directory
# The error message still appears because stderr is not piped
$ ls /nonexistent 2>/dev/null
# No output - stderr was redirected to /dev/null
```

Shell нь эдгээр урсгалуудыг чиглүүлэх (redirecting) синтаксийг санал болгодог. Жишээнүүд:

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

Unix философийг сайн харуулдаг өөр нэг хүчтэй хэрэгсэл бол [`fzf`](https://github.com/junegunn/fzf) fuzzy finder юм. Энэ нь `stdin`-ээс мөрүүд уншаад, шүүж сонгох интерактив интерфейс гаргаж өгдөг:

```console
$ ls | fzf
$ cat ~/.bash_history | fzf
```

`fzf` нь shell-ийн олон үйлдлүүдтэй нэгдэн ажиллах боломжтой. Shell-ийг өөрчлөх хэсэгт бид үүнийг хэрхэн ашиглах талаар дэлгэрэнгүй үзэх болно.

## Орчны хувьсагчид

Bash-д хувьсагч оноохдоо `foo=bar` гэсэн синтакс ашиглах ба хувьсагчийн утгыг авахдаа `$foo` гэж бичнэ. `foo = bar` гэж бичих нь буруу синтакс бөгөөд shell үүнийг `foo` програмыг `['=', 'bar']` аргументуудтай дуудаж байна гэж задлан шинжлэх болно. Shell скрипт дээр хоосон зайны үүрэг нь аргументуудыг салгах явдал байдаг. Энэ ажиллагаа нь эхэндээ гэнэтийн алдаа дагуулж болох тул үүнийг анхаарна уу.

Shell хувьсагчид төрөл гэж байхгүй бөгөөд бүгд тэмдэгт мөр байдаг. Shell дээр тэмдэгт мөр бичихэд дан хашилт болон давхар хашилтын тэмдэгтүүд өөр өөр үүрэгтэй гэдгийг санаарай. `'`-оор хашсан тэмдэгт мөр нь хувьсагчийг задлахгүй, команд орлуулахгүй, мөн тусгай тэмдэгтүүдийг хөрвүүлэхгүйгээр шууд утгыг хадгалдаг бол `"`-оор хашсан тэмдэгт мөр нь эдгээрийг ажиллуулж хувиргадаг.

```shell
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```

Командын гаралтыг хувьсагчид хадгалахын тулд бид *command substitution*-ыг ашигладаг. Бид дараах кодыг ажиллуулахад:

```shell
files=$(ls)
echo "$files" | grep README
echo "$files" | grep ".py"
```

ls-ийн гаралт (тодруулбал stdout) `$files` хувьсагчид хадгалагдах бөгөөд бид үүнийг дараа нь ашиглаж болно. `$files` хувьсагчийн агуулга нь ls-ийн гаралтын шинэ мөр шилжүүлэх тэмдэгтүүдийг хадгалдаг тул `grep` зэрэг програмууд мөр бүр дээр бие даан ажиллах боломжтой байдаг.

Үүнтэй төстэй, арай бага дурдагддаг өөр нэг боломж бол *process substitution* бөгөөд `<( CMD )` нь `CMD` командыг ажиллуулж, гаралтыг нь түр зуурын файлд хадгалаад уг түр файлын нэрийг `<()`-ийн оронд орлуулан тавьдаг. Энэ нь програмууд утгыг STDIN-ээр биш харин файлаар дамжуулан авахыг хүлээж байгаа үед их хэрэг болдог. Жишээлбэл, `diff <(ls src) <(ls docs)` нь `src` болон `docs` folder доторх файлуудын ялгааг харуулах болно.

Shell-ээс өөр нэг програмыг ажиллуулах бүрд тэрээр *environment variables* (орчны хувьсагчид) гэж нэрлэгддэг хувьсагчдын багцыг дамжуулдаг. Бид shell дээр `printenv` командыг ажиллуулан одоогийн environment variables-ийг харж болно. Environment variable-ийг тодорхой програмд зориулан дамжуулахын тулд командын өмнө хувьсагч оноох үйлдлийг бичнэ:

> Орчны хувьсагчдыг ТОМ ҮСГЭЭР (жишээ нь, `HOME`, `PATH`, `DEBUG`) бичих журамтай. Энэ нь техникийн шаардлага биш боловч локал хувьсагчдаас (ихэвчлэн жижиг үсгээр бичигддэг) ялгахад тусалдаг.

```shell
TZ=Asia/Tokyo date  # prints the current time in Tokyo
echo $TZ  # this will be empty, since TZ was only set for the child command
```

Үүний оронд бид одоогийн орчныг өөрчлөх `export` суурилагдсан функцийг ашиглаж болох бөгөөд ингэснээр дараах бүх хүү процессууд уг хувьсагчийг өвлөн авах болно:

```shell
export DEBUG=1
# All programs from this point onwards will have DEBUG=1 in their environment
bash -c 'echo $DEBUG'
# prints 1
```

Хувьсагчийг устгахын тулд shell-ийн built-in `unset` командыг ашиглана, жишээ нь `unset DEBUG`.

> Орчны хувьсагчид нь shell-ийн өөр нэгэн дүрэм юм. Тэдгээрийг олон програмын ажиллагааг шууд бусаар өөрчлөхөд ашиглаж болно. Жишээлбэл, shell нь одоогийн хэрэглэгчийн home folder-ийн замыг `$HOME` орчны хувьсагчид хадгалдаг. Ингэснээр програмууд `--home /home/alice` гэж зааж өгөхийг шаардалгүйгээр энэ хувьсагчаас мэдээллийг авч чаддаг. Өөр нэг жишээ бол олон програмууд огноо, цагийг форматыг тодорхойлоход ашигладаг `$TZ` хувьсагч юм.

## Гаралтын кодууд

Бидний өмнө үзсэнчлэн, shell програмын үндсэн гаралт нь stdout/stderr урсгалууд болон файлын систем дэх өөрчлөлтүүдээр илэрхийлэгддэг.

Анхны тохиргоогоор shell скрипт нь гаралтын код (exit code) тэг утгыг буцаадаг. Дүрмийн дагуу тэг гаралтын код нь бүх зүйл амжилттай болсныг илтгэдэг бол тэгээс өөр утга нь алдаа гарсан болохыг илэрхийлнэ. Тэгээс өөр код буцаахын тулд бид shell-ийн `exit NUM` суурилагдсан командыг ашиглах ёстой. Хамгийн сүүлд ажилласан командын гаралтын кодыг бид тусгай `$?` хувьсагчаар дамжуулан авч болно.

Shell-д AND болон OR үйлдлүүдийг гүйцэтгэх `&&` болон `||` логик операторууд байдаг. Ердийн програмчлалын хэлнээс ялгаатай нь shell дэх эдгээр операторууд нь програмуудын гаралтын кодууд дээр ажилладаг. Тэд хоёулаа [богино холболтын](https://en.wikipedia.org/wiki/Short-circuit_evaluation) (short-circuiting) операторууд юм. Энэ нь өмнөх командын амжилттай (код 0) эсвэл амжилтгүй (тэгээс өөр код) болсон дээр үндэслэн дараагийн командуудыг нөхцөлтэйгөөр ажиллуулахад ашиглагдаж болно гэсэн үг юм. Жишээнүүд:

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

Үүнтэй ижил зарчим `if` болон `while` нөхцөлүүдэд хамаарна, тэд хоёулаа гаралтын кодуудыг ашиглан шийдвэр гаргадаг:

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

## Сигналууд

Зарим тохиолдолд та ажиллаж байгаа програмыг зогсоох шаардлагатай болдог, жишээлбэл команд хэтэрхий удаан ажиллах үед. Програмыг зогсоох хамгийн энгийн арга бол `Ctrl-C` дарах бөгөөд үүний дараа команд зогсох болно. Гэхдээ энэ нь цаанаа хэрхэн ажилладаг, яагаад заримдаа процессыг зогсоож чаддаггүй вэ?

```console
$ sleep 100
^C
$
```

> Энд `^C` нь терминал дээр `Ctrl-C` дарах үед харагдах тэмдэглэгээ юм.

Цаанаа дараах үйл явц өрнөсөн:

1. Бид `Ctrl-C` дарсан
2. Shell энэ тусгай тэмдэгтүүдийг таньсан
3. Shell процесс `sleep` процесс руу `SIGINT` сигналыг илгээсэн
4. Сигнал нь `sleep` процессын ажиллагааг зогсоосон

Сигналууд (Signals) нь тусгай харилцааны механизм юм. Процесс сигнал хүлээн авах үедээ ажиллагаагаа зогсоож, сигналыг боловсруулаад, сигналын өгсөн мэдээлэлд үндэслэн ажиллагааны урсгалаа өөрчлөх боломжтой байдаг. Ийм учраас сигналуудыг *програм хангамжийн тасалдал* (software interrupts) гэж нэрлэдэг.

Бидний хувьг `Ctrl-C` дарах нь shell-ийг процесс руу `SIGINT` сигналыг илгээхэд хүргэдэг. `SIGINT`-ийг барьж аваад үл тоомсорлодог, ажиллагаагаа зогсоодоггүй Python програмын жижиг жишээг доор үзүүлэв. Энэ програмыг зогсоохын тулд бид `Ctrl-\` дарж `SIGQUIT` сигналыг илгээх хэрэгтэй.

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

Энэ програмд `SIGINT`-ийг хоёр удаа илгээж, дараа нь `SIGQUIT` илгээхэд юу болохыг доор үзүүлэв. `^` нь терминал дээр `Ctrl` товчлуурыг илэрхийлнэ.

```console
$ python sigint.py
24^C
I got a SIGINT, but I am not stopping
26^C
I got a SIGINT, but I am not stopping
30^\[1]    39913 quit       python sigint.py
```

`SIGINT` болон `SIGQUIT` нь терминалтай холбоотой хүсэлтүүдэд ашиглагддаг бол процессыг аюулгүй, зөв зогсоох ерөнхий сигнал бол `SIGTERM` юм. Энэ сигналыг илгээхийн тулд бид [`kill`](https://www.man7.org/linux/man-pages/man1/kill.1.html) командыг `kill -TERM <PID>` синтакс ашиглан дуудаж болно.

Сигнал нь процессыг устгахаас гадна өөр зүйлсийг хийж чадна. Жишээлбэл, `SIGSTOP` нь процессыг түр зогсооно. Терминал дээр `Ctrl-Z` дарах нь shell-ийг `SIGTSTP` (Terminal Stop - терминалын `SIGSTOP` хувилбар) сигнал илгээхэд хүргэдэг.

Бид түр зогссон ажлыг [`fg`](https://www.man7.org/linux/man-pages/man1/fg.1p.html) эсвэл [`bg`](https://man7.org/linux/man-pages/man1/bg.1p.html) command-уудаар харгалзан урд талд (foreground) эсвэл ард (background) үргэлжлүүлэн ажиллуулж болно.

[`jobs`](https://www.man7.org/linux/man-pages/man1/jobs.1p.html) command нь одоогийн терминалын сесстэй холбоотой дуусаагүй байгаа ажлуудыг жагсаадаг. Та эдгээр ажлуудыг pid-ээр нь зааж өгч болно (pid-ийг олохын тулд [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html)-ийг ашиглана). Мөн процессыг хувийн тэмдэгт `%` болон ажлын дугаараар (jobs command-ын харуулдаг дугаар) зааж өгөх боломжтой. Хамгийн сүүлд ард ажиллуулсан ажлыг тусгай `$!` параметрээр авна.

Командын төгсгөлд `&` залгаж бичвэл command-ыг шууд ард ажиллуулж, prompt-ийг танд буцааж өгөх болно. Гэхдээ энэ нь shell-ийн STDOUT-ийг ашигласаар байх тул гаралт хэвлэгдээд төвөгтэй байж болно (үүнийг шийдэхийн тулд гаралтыг чиглүүлнэ үү). Үүнтэй ижил үр дүнд хүрэхийн тулд ажиллаж байгаа програмыг `Ctrl-Z` дарж зогсоогоод дараа нь `bg` гэж ажиллуулж болно.

Ард ажиллаж байгаа процессууд нь таны терминалын хүү процессууд хэвээр байх бөгөөд хэрэв та терминалыг хаавал тэд устгагдах болно (терминал хаагдахад тэдэнд `SIGHUP` сигнал очих болно). Үүнээс сэргийлэхийн тулд програмыг [`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html) (SIGHUP сигналыг үл тоомсорлогч wrapper) ашиглан ажиллуулах эсвэл ажиллаж эхэлсэн процессыг `disown` command-аар салгаж болно. Эсвэл дараагийн хэсэгт үзэх терминал үржүүлэгчийг ашиглаж болно.

Доорх жишээнд эдгээр ойлголтуудыг харуулав.

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

`SIGKILL` нь тусгай сигнал бөгөөд үүнийг процесс барьж авч чадахгүй тул процессыг шууд зогсоодог. Гэхдээ энэ нь системд өнчин хүү процессуудыг үлдээх зэрэг сөрөг үр дагавартай байж болно.

Та эдгээр болон бусад сигналуудын талаар [эндээс](https://en.wikipedia.org/wiki/Signal_(IPC)) эсвэл [`man signal`](https://www.man7.org/linux/man-pages/man7/signal.7.html), `kill -l` командаар дэлгэрүүлж үзнэ үү.

Shell скрипт дотор сигнал хүлээн авах үед командуудыг ажиллуулахын тулд `trap` суурилагдсан командыг ашиглаж болно. Энэ нь цэвэрлэгээ хийх үйлдлүүдэд их хэрэг болдог:

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

# Алсын серверүүд

Өнөө үед програмчид өдөр тутмын ажилдаа алсын серверүүдтэй ажиллах нь улам бүр түгээмэл болж байна. Үүний тулд хамгийн түгээмэл ашиглагддаг хэрэгсэл бол SSH (Secure Shell) бөгөөд энэ нь алсын сервертэй холбогдож танил shell интерфейсийг олгодог. Бид сервертэй дараах командаар холбогдоно:

```bash
ssh alice@server.mit.edu
```

Энд бид `server.mit.edu` сервер рүү `alice` хэрэглэгчээр нэвтрэхийг оролдож байна.

`ssh`-ийн хүмүүс тэр бүр анзаардаггүй нэг давуу тал нь command-ыг интерактив бус байдлаар ажиллуулж болдогт байдаг. `ssh` нь command-ын stdin-ийг дамжуулж, stdout-ийг буцааж авах ажлыг зөв хийдэг тул бусад command-тай амархан хослуулж болно:

```shell
# here ls runs in the remote, and wc runs locally
ssh alice@server ls | wc -l

# here both ls and wc run in the server
ssh alice@server 'ls | wc -l'

```

> Сүлжээ тасрах, унтах горимд шилжих, сүлжээ солигдох болон сүлжээний хоцролт ихтэй үед найдвартай ажилладаг SSH-ийг орлох [Mosh](https://mosh.org/) хэрэгслийг суулгаад ашиглаад үзээрэй.

Remote server дээр command ажиллуулахын тулд `ssh` таныг зөвшөөрөлтэй хэрэглэгч мөн гэдгийг баталгаажуулах ёстой. Үүнийг нууц үгээр эсвэл SSH түлхүүрээр хийж болно. Түлхүүрт суурилсан authentication нь public-key криптограф ашигладаг тул нууц private key-гээ ил гаргалгүйгээр server-т өөрийгөө таниулна. Энэ арга нь илүү эвтэйхэн, бас аюулгүй тул боломжтой бол үүнийг ашигласан нь дээр. Private key (ихэвчлэн `~/.ssh/id_rsa`, сүүлийн үед `~/.ssh/id_ed25519`) нь нууц үгтэй адил мэдрэмтгий зүйл тул сайн хамгаалж, хэзээ ч бусадтай хуваалцаж болохгүй.

Түлхүүрийн хослолыг үүсгэхийн тулд [`ssh-keygen`](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html) ажиллуулна.
```bash
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519
```

Хэрэв та GitHub руу SSH түлхүүр ашиглан ханддаг бол [энд](https://help.github.com/articles/connecting-to-github-with-ssh/) дурдсан алхмуудыг аль хэдийн хийсэн бөгөөд хүчинтэй түлхүүрийн хослолтой байгаа байх. Түлхүүрийн нууц үгийг шалгаж, баталгаажуулахын тулд `ssh-keygen -y -f /path/to/key` командыг ажиллуулж болно.

Сервер тал дээр `ssh` нь аль үйлчлүүлэгчийг нэвтрүүлэхээ шийдэхийн тулд `.ssh/authorized_keys` файлыг шалгадаг. Өөрийн нийтийн түлхүүрийг сервер рүү хуулахын тулд:

```bash
cat .ssh/id_ed25519.pub | ssh alice@remote 'cat >> ~/.ssh/authorized_keys'

# or more simply (if ssh-copy-id is available)

ssh-copy-id -i .ssh/id_ed25519 alice@remote
```

Command ажиллуулахаас гадна ssh-ийн нээсэн холболтоор файлаа server рүү, эсвэл server-ээс локал руу найдвартай зөөж болно. [`scp`](https://www.man7.org/linux/man-pages/man1/scp.1.html) нь уламжлалт хэрэгсэл бөгөөд syntax нь `scp path/to/local_file remote_host:path/to/remote_file`. [`rsync`](https://www.man7.org/linux/man-pages/man1/rsync.1.html) нь local болон remote талд аль файл аль хэдийн ижил байгааг таньж, дахин хуулахгүй гэдгээрээ `scp`-ээс давуу. Мөн symlink, permission удирдах, тасарсан хуулалтыг үргэлжлүүлэх `--partial` flag гэх мэт нэмэлт боломжтой. `rsync` нь `scp`-тэй төстэй syntax ашигладаг.

SSH үйлчлүүлэгчийн тохиргоо `~/.ssh/config` файлд байрлах бөгөөд энд бид серверүүд болон тэдгээрийн анхны тохиргоог тодорхойлж болно. Энэ тохиргооны файлыг зөвхөн `ssh` бус, мөн `scp`, `rsync`, `mosh` зэрэг бусад програмууд уншиж ашигладаг.

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

# Терминал үржүүлэгчид

CLI интерфейсийг ашиглах үед та нэгэн зэрэг олон зүйл ажиллуулах шаардлагатай болдог. Жишээлбэл, код засварлагч болон програмаа зэрэгцүүлж харахыг хүсэж болно. Үүнийг олон терминалын цонх нээж хийж болох ч terminal multiplexer ашиглах нь илүү тохиромжтой шийдэл юм.

[`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) зэрэг terminal multiplexers нь терминалын цонхыг хэсгүүд (panes) болон таб (windows) болгон хувааж ашиглах боломжийг олгосноор олон shell сессүүдтэй үр дүнтэй ажиллах нөхцөлийг бүрдүүлдэг. Түүнчлэн, terminal multiplexer нь одоогийн терминалын сессийг detach хийгээд, дараа нь reattach хийх боломж өгдөг. Үүнээс шалтгаалан terminal multiplexers нь алсын сервертэй ажиллах үед `nohup` зэрэг аргуудыг ашиглах шаардлагагүй болгодог тул маш тохиромжтой байдаг.

Өнөө үед хамгийн алдартай terminal multiplexer бол [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) юм. `tmux` нь тохируулах өндөр боломжтой бөгөөд холбогдох товчны хослолуудыг ашиглан олон таб, хэсгүүдийг үүсгэж тэдгээрийн хооронд хурдан шилжиж болно.

`tmux`-ийг ашиглахын тулд түүний товчны хослолуудыг мэдэх шаардлагатай бөгөөд тэдгээр нь бүгд `<C-b> x` хэлбэртэй байна: (1) `Ctrl+b` дарж, (2) `Ctrl+b`-ийг суллаад, (3) `x` товчлуурыг дарна. `tmux` нь дараах шатлал бүхий объектуудтай:
- **Сессүүд (Sessions)** - сесс нь нэг буюу хэд хэдэн цонх агуулсан бие даасан ажлын талбар юм.
    + `tmux` нь шинэ сесс эхлүүлнэ.
    + `tmux new -s NAME` нь сессийг нэр өгч эхлүүлнэ.
    + `tmux ls` нь одоо байгаа идэвхтэй сессүүдийг жагсаана.
    + `tmux` дотор `<C-b> d` дарах нь одоогийн сессийг салгана.
    + `tmux a` нь хамгийн сүүлийн сесстэй эргэж холбогдоно. Сонгож холбогдохын тулд `-t` аргумент ашиглана.

- **Цонхнууд (Windows)** - Засварлагч эсвэл хөтөч дээрх табтай адил бөгөөд нэг сессийн доторх тусдаа хэсгүүд юм.
    + `<C-b> c` Шинэ цонх үүсгэнэ. Хаахын тулд shell-ийг `<C-d>` дарж хаахад хангалттай.
    + `<C-b> N` *N* дэх цонх руу шилжинэ. Цонхнууд дугаарлагдсан байдгийг анхаарна уу.
    + `<C-b> p` Өмнөх цонх руу шилжинэ.
    + `<C-b> n` Дараагийн цонх руу шилжинэ.
    + `<C-b> ,` Одоогийн цонхны нэрийг өөрчилнө.
    + `<C-b> w` Одоогийн цонхнуудыг жагсаана.

- **Хэсгүүд (Panes)** - Vim хуваалттай адил бөгөөд нэг дэлгэц дээр олон shell зэрэг ажиллуулах боломжийг олгоно.
    + `<C-b> "` Одоогийн хэсгийг хөндлөн хуваана.
    + `<C-b> %` Одоогийн хэсгийг босоо хуваана.
    + `<C-b> <direction>` Заасан *direction* рүү шилжинэ. Direction гэдэг нь сумтай товчлуурууд юм.
    + `<C-b> z` Одоогийн хэсгийг дэлгэц дүүргэх байдлаар томруулна / жижигсгэнэ.
    + `<C-b> [` Гүйлгэж харах горимыг эхлүүлнэ. Та `<space>` дарж сонгож эхлээд, `<enter>` дарж сонгосон хэсгийг хуулж авч болно.
    + `<C-b> <space>` Хэсгүүдийн байршлыг сольж өөрчилнө.

> tmux-ийн талаар илүү iихийг мэдэхийн тулд [энэ](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) богино заавар болон [энэ](https://linuxcommand.org/lc3_adv_termmux.php) дэлгэрэнгүй тайлбарыг уншина уу.

tmux болон SSH ашиглаж сурсны дараа та өөрийн ажиллах орчныг дурын машин дээр өөрийнхөөрөө байлгахыг хүсэх болно. Энд shell тохиргоо (shell customization) хэрэг болно.

# Shell-ийг өөртөө тааруулан өөрчлөх

Олон CLI програм *dotfiles* гэж нэрлэдэг plain-text файлуудаар тохируулагддаг. Файлын нэр нь `.`-оор эхэлдэг тул (`~/.vimrc` гэх мэт) ийм файлууд `ls` хийхэд default-аар харагддаггүй, hidden байдаг.

> Dotfiles нь shell-ийн өөр нэгэн дүрэм юм. Эхэнд байгаа цэг нь тэдгээрийг жагсаалтад харуулахгүй нуух зорилготой байдаг.

Shell-үүд нь ийм файлуудаар тохируулагддаг програмуудын нэг жишээ юм. Ажиллаж эхлэхдээ shell нь тохиргоогоо ачаалахын тулд олон файл уншдаг. Shell-ийн төрөл болон нэвтрэх сесс (login session) эсвэл интерактив сессээс хамааран энэ үйл явц нь нэлээд нарийн төвөгтэй байж болно. Сэдвийн талаарх маш сайн нөөцийг [эндээс](https://web.archive.org/web/20260329133158/https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html) үзнэ үү.

`bash`-ийн хувьд `.bashrc` эсвэл `.bash_profile` файлыг засварлах нь ихэнх системд тохирдог. Dotfiles ашиглан тохируулж болох зарим хэрэгслүүд:

- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` болон `~/.vim` folder
- `ssh` - `~/.ssh/config`
- `tmux` - `~/.tmux.conf`

Түгээмэл хийгддэг тохиргооны өөрчлөлт бол shell програм хайх шинэ байршлуудыг нэмэх юм. Програм хангамж суулгах үед та энэ загвартай таарах болно:

```shell
export PATH="$PATH:path/to/append"
```

Энд бид shell-д $PATH хувьсагчийн утгыг одоогийн утга дээр шинэ замыг нэмж оноож, бүх хүү процессууд PATH-ийн энэ шинэ утгыг өвлөн авахыг зааж байна. Энэ нь хүү процессуудад `path/to/append` folder доор байрлах програмуудыг олох боломж өгнө.

Shell-ээ өөрчлөх нь ихэвчлэн шинэ CLI хэрэгслүүдийг суулгахыг шаарддаг. Багц удирдагчид (package managers) үүнийг хялбар болгодог. Тэд програм татах, суулгах, шинэчлэх үйлдлүүдийг хариуцдаг. Үйлдлийн систем бүр өөр өөрийн багц удирдагчтай байдаг: macOS нь [Homebrew](https://brew.sh/), Ubuntu/Debian нь `apt`, Fedora нь `dnf`, харин Arch нь `pacman` ашигладаг. Бид багц удирдагчдын талаар кодын тээвэрлэлт лекцээр дэлгэрэнгүй үзэх болно.

macOS дээр Homebrew ашиглан хоёр хэрэгтэй програмыг хэрхэн суулгахыг доор үзүүлэв:

```shell
# ripgrep: a faster grep with better defaults
brew install ripgrep

# fd: a faster, user-friendly find
brew install fd
```

Эдгээрийг суулгаснаар та `grep`-ийн оронд `rg`, харин `find`-ын оронд `fd` командыг ашиглаж болно.

> **`curl | bash` анхааруулга**: Та `curl -fsSL https://example.com/install.sh | bash` гэх мэт суулгах зааврыг олноор харах болно. Энэ арга нь скриптийг татаж аваад шууд ажиллуулдаг тул маш хялбар боловч эрсдэлтэй; та шалгаагүй кодоо шууд ажиллуулж байна гэсэн үг юм. Илүү аюулгүй арга бол эхлээд татаж аваад, кодыг шалгаад дараа нь ажиллуулах юм:
> ```shell
> curl -fsSL https://example.com/install.sh -o install.sh
> less install.sh  # review the script
> bash install.sh
> ```
> Some installers use a slightly safer variant: `/bin/bash -c "$(curl -fsSL https://url)"` which at least ensures bash interprets the script rather than your current shell.

When you try to run a command that isn't installed, your shell will show `command not found`. The website [command-not-found.com](https://command-not-found.com) is a helpful resource you can use to search for any command to find out how to install it across different package managers and distributions.

Another useful tool is [`tldr`](https://tldr.sh/), which provides simplified, example-focused man pages. Instead of reading through lengthy documentation, you can quickly see common usage patterns:

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

Sometimes you don't need a whole new program, but rather just a shortcut for an existing command with specific flags. That's where aliases come in.

We can also create our own command aliases using the `alias` shell built-in.
A shell alias is a short form for another command that your shell will replace automatically before evaluating the expression.
For instance, an alias in bash has the following structure:

```bash
alias alias_name="command_to_alias arg1 arg2"
```

> Note that there is no space around the equal sign `=`, because [`alias`](https://www.man7.org/linux/man-pages/man1/alias.1p.html) is a shell command that takes a single argument.

Aliases have many convenient features:

```bash
# Түгээмэл flags-д зориулсан товчлол үүсгэх
alias ll="ls -lh"

# Байнга бичдэг урт command-уудыг хурдан бичих
alias gs="git status"
alias gc="git commit"

# Буруу бичихээс сэргийлэх
alias sl=ls

# Програмуудыг илүү найдвартай тохиргоотой болгох
alias mv="mv -i"           # -i нь файлыг дахин бичихээс өмнө асууна
alias mkdir="mkdir -p"     # -p нь эцэг folder байхгүй бол үүсгэнэ
alias df="df -h"           # -h нь хэмжээг хүнд уншихад хялбар форматоор харуулна

# Алиасуудыг хослуулж болно
alias la="ls -A"
alias lla="la -l"

# Алиасыг үл тоомсорлон жинхэнэ command-ыг ажиллуулахдаа өмнө нь \ тавина
\ls
# Эсвэл unalias ашиглан алиасыг бүрмөсөн идэвхгүй болгоно
unalias la

# Алиасын тодорхойлолтыг харахын тулд alias-ийг дуудна
alias ll
# ll='ls -lh' гэж хэвлэнэ
```

Aliases have limitations: they cannot take arguments in the middle of a command. For more complex behavior, you should use shell functions instead.

Most shells support `Ctrl-R` for reverse history search. Type `Ctrl-R` and start typing to search through previous commands. Earlier we introduced `fzf` as a fuzzy finder; with fzf's shell integration configured, `Ctrl-R` becomes an interactive fuzzy search through your entire history, far more powerful than the default.

How should you organize your dotfiles? They should be in their own folder,
under version control, and **symlinked** into place using a script. This has
the benefits of:

- **Easy installation**: if you log in to a new machine, applying your
customizations will only take a minute.
- **Portability**: your tools will work the same way everywhere.
- **Synchronization**: you can update your dotfiles anywhere and keep them all
in sync.
- **Change tracking**: you're probably going to be maintaining your dotfiles
for your entire programming career, and version history is nice to have for
long-lived projects.

What should you put in your dotfiles?
You can learn about your tool's settings by reading online documentation or
[man pages](https://en.wikipedia.org/wiki/Man_page). Another great way is to
search the internet for blog posts about specific programs, where authors will
tell you about their preferred customizations. Yet another way to learn about
customizations is to look through other people's dotfiles: you can find tons of
[dotfiles
repositories](https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories)
on GitHub --- see the most popular one
[here](https://github.com/mathiasbynens/dotfiles) (we advise you not to blindly
copy configurations though).
[Here](https://dotfiles.github.io/) is another good resource on the topic.

All of the class instructors have their dotfiles publicly accessible on GitHub: [Anish](https://github.com/anishathalye/dotfiles),
[Jon](https://github.com/jonhoo/configs),
[Jose](https://github.com/jjgo/dotfiles).

**Frameworks and plugins** can improve your shell as well. Some popular general frameworks are [prezto](https://github.com/sorin-ionescu/prezto) or [oh-my-zsh](https://ohmyz.sh/), and smaller plugins that focus on specific features:

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) - colors valid/invalid commands as you type
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) - suggests commands from history as you type
- [zsh-completions](https://github.com/zsh-users/zsh-completions) - additional completion definitions
- [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search) - fish-like history search
- [powerlevel10k](https://github.com/romkatv/powerlevel10k) - fast, customizable prompt theme

Shells like [fish](https://fishshell.com/) include many of these features by default.

> You don't need a massive framework like oh-my-zsh to get these features. Installing individual plugins is often faster and gives you more control. Large frameworks can significantly slow down shell startup time, so consider installing only what you actually use.


# AI in the Shell

There are many ways to incorporate AI tooling in the shell. Here are a few examples at different levels of integration:

**Command generation**: Tools like [`simonw/llm`](https://github.com/simonw/llm) can help generate shell commands from natural language descriptions:

```console
$ llm cmd "find all python files modified in the last week"
find . -name "*.py" -mtime -7
```

**Pipeline integration**: LLMs can be integrated into shell pipelines to process and transform data. They're particularly useful when you need to extract information from inconsistent formats where regex would be painful:

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

Бид хувьсагч дотор хоосон зай байгаа тул `"$INSTRUCTIONS"` (хашилттай) ашиглаж байгааг болон файлын агуулгыг stdin рүү чиглүүлэхийн тулд `< users.txt`-ийг ашиглаж байгааг анхаарна уу.

**AI shell-үүд**: [Claude Code](https://docs.anthropic.com/en/docs/claude-code) зэрэг хэрэгслүүд нь англи хэлээр өгсөн зааврыг shell-ийн үйлдэл, файл засварлах болон олон алхамт нарийн төвөгтэй ажлууд болгон хөрвүүлдэг meta-shell-ийн үүргийг гүйцэтгэдэг.

# Терминал эмуляторууд

Shell-ээ тохируулахаас гадна өөрийн ашиглах **terminal emulator** (терминал эмулятор) болон түүний тохиргоог сонгоход хэсэг хугацаа зарцуулах нь зүйтэй. Terminal emulator нь таны shell ажилладаг текстэн интерфейсийг олгодог GUI програм юм. Маш олон terminal emulators байдаг.

Терминал дээрээ олон зуун, магадгүй олон мянган цагийг зарцуулах тул түүний тохиргоог анхаарах нь үр өгөөжтэй байдаг. Та терминал дээрээ дараах зүйлсийг өөрчилж болно:

- Шрифтийн сонголт (Font selection)
- Өнгөний зохицол (Color scheme)
- Гарын товчлолууд (Keyboard shortcuts)
- Таб/Хэсгийн дэмжлэг (Tabs/Panes support)
- Гүйлгэж харах тохиргоо (Scrollback configuration)
- Хурд, гүйцэтгэл (үүний тулд [Alacritty](https://github.com/alacritty/alacritty) эсвэл [Ghostty](https://ghostty.org/) зэрэг GPU хурдатгалтай шинэ терминалуудыг ашиглаж болно).

# Дасгал ажил

## Аргументууд ба globs

1. Та заримдаа `cmd --flag -- --notaflag` гэх мэт командуудыг харах болно. `--` нь програмд flag задлан шинжлэхээ зогсоохыг заадаг тусгай аргумент юм. `--`-ийн дараа байгаа бүх зүйлийг байрлалын аргумент (positional argument) гэж үзнэ. Энэ нь яагаад хэрэгтэй байж болох вэ? `touch -- -myfile` гэж ажиллуулан файл үүсгээд, дараа нь `--` ашиглахгүйгээр устгахыг оролдоно уу.

1. [`man ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html) хуудсыг уншаад ингэж файлуудыг жагсаах `ls` команд бичнэ үү:
    - Нууц файлыг оролцуулан бүх файлыг харуулна
    - Хэмжээг хүнд уншихад хялбар форматоор харуулна (жишээ нь 454279954-ийн оронд 454M)
    - Файлуудыг сүүлд өөрчлөгдсөн хугацаагаар нь эрэмбэлнэ
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

1. Process substitution `<(command)` нь command-ын гаралтыг файл шиг ашиглах боломж өгдөг. Process substitution-тай `diff` ашиглан `printenv` болон `export`-ийн гаралтыг харьцуулна уу. Тэд юугаараа ялгаатай вэ? (Сэжүүр: `diff <(printenv | sort) <(export | sort)` ажиллуулна уу).

## Environment Variables

1. Дараах үйлдлийг гүйцэтгэх bash функцууд болох `marco` болон `polo`-г бичнэ үү: таныг `marco` ажиллуулах бүрд одоогийн ажиллаж буй folder хадгалагдах бөгөөд таныг ямар ч folder дотор байсан `polo` ажиллуулахад `cd` хийж таныг `marco`-г ажиллуулсан folder руу буцаах болно. Алдааг хялбар засахын тулд кодыг `marco.sh` файлд бичиж, `source marco.sh` ажиллуулан өөрийн shell-д ачаалж туршина уу.

{% comment %}
marco() {
    export MARCO=$(pwd)
}
polo() {
    cd "$MARCO"
}
{% endcomment %}

## Return Codes

1. Танд хааяа нэг алдаа заадаг команд байна гэж бодъё. Алдааг илрүүлэхийн тулд гаралтыг хадгалах шаардлагатай боловч алдаа заах хүртэл гараар ажиллуулах нь цаг их үрнэ. Дараах скриптийг алдаа заах хүртэл нь ажиллуулах бөгөөд түүний стандарт гаралт болон алдааны урсгалуудыг файлд хадгалж, төгсгөлд нь бүх гаралтыг хэвлэдэг bash скрипт бичнэ үү. Хэрэв скрипт алдаа заах хүртэл нийт хэдэн удаа ажилласныг хэвлэж чадвал бонус оноо болно.

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

## Signals and Job Control

1. Терминал дээр `sleep 10000` ажлыг эхлүүлээд, `Ctrl-Z` дарж түр зогсоон `bg` command-аар ард ажиллуулна уу. Одоо [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) ашиглан түүний pid-ийг олж, [`pkill`](https://man7.org/linux/man-pages/man1/pgrep.1.html) ашиглан pid-ийг гараар бичихгүйгээр устгана уу. (Сэжүүр: `-lf` flags ашиглана уу).

1. Та нэг процессыг дуустал өөр процессыг ажиллуулахгүй хүлээхийг хүсэж байна гэж бодъё. Үүнийг хэрхэн шийдэх вэ? Энэ дасгалд хүлээх процесс маань үргэлж `sleep 60 &` байна. Үүнд хүрэх нэг арга бол [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) командыг ашиглах юм. Sleep командыг ажиллуулаад, `ls` командыг арын процесс дуустал хүлээдэг болгон туршина уу.

    Гэвч, хэрэв бид өөр багц сессээс эхлүүлбэл энэ арга ажиллахгүй, учир нь `wait` нь зөвхөн хүү процессууд дээр ажилладаг. Тэмдэглэлд дурдаагүй нэг зүйл бол `kill` командын гаралтын төлөв амжилттай бол 0, үгүй бол тэгээс өөр утга буцаадаг юм. `kill -0` нь сигнал илгээдэггүй боловч процесс байхгүй тохиолдолд тэгээс өөр код буцаана. PID авдаг бөгөөд уг процесс дуусах хүртэл хүлээдэг `pidwait` нэртэй bash функц бичнэ үү. CPU-ийг дэмий үрэхээс сэргийлэхийн тулд `sleep` ашиглах хэрэгтэй.

## Files and Permissions

1. (Гүнзгийрүүлсэн) Directory дотроос хамгийн сүүлд өөрчлөгдсөн файлыг recursive байдлаар олдог command эсвэл script бичнэ үү. Илүү ерөнхий байдлаар, бүх файлыг өөрчлөгдсөн хугацаагаар нь жагсааж чадах уу?

## Terminal Multiplexers

1. Энэ `tmux` [зааврыг](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) дагаж суралцаад, дараа нь [эдгээр алхмуудын](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/) дагуу үндсэн тохиргоонуудыг хийж сурна уу.

## Aliases and Dotfiles

1. Командыг буруу бичих үед зориулан `cd` рүү хөрвөх `dc` алиасыг (alias) үүсгэнэ үү.

1. `history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10` ажиллуулан өөрийн хамгийн их ашигласан 10 командыг олж, тэдгээрт зориулсан богино алиас бичих талаар бодно уу. Тэмдэглэл: энэ нь Bash дээр ажиллана; хэрэв та ZSH ашиглаж байгаа бол зүгээр `history` биш `history 1` ашиглана уу.

1. Өөрийн dotfiles-д зориулж folder үүсгэн хувилбарын хяналтад (Git) оруулна уу.

1. Ядаж нэг програмын тохиргоог (эхлэхдээ shell prompt-ийг өөрчлөх `$PS1` тохиргоо байж болно) өөрийн dotfiles-д нэмнэ үү.

1. Өөрийн dotfiles-ыг шинэ машин дээр хурдан (мөн механик ажиллагаагүй) суулгах аргыг бэлтгэнэ үү. Энэ нь файл бүрд `ln -s` хийдэг энгийн скрипт эсвэл [тусгай хэрэгсэл](https://dotfiles.github.io/utilities/) ашигласан байж болно.

1. Суулгах скриптээ шинэ виртуал машин дээр туршиж үзнэ үү.

1. Өөрийн одоогийн ашиглаж буй хэрэгслүүдийн бүх тохиргоог dotfiles сан руугаа шилжүүлнэ үү.

1. Өөрийн dotfiles-оо GitHub дээр нийтэлнэ үү.

## Remote Machines (SSH)

Эдгээр дасгалд зориулж Linux virtual machine суулгаарай (эсвэл аль хэдийн байгаа нэгийг ашиглаж болно). Virtual machine сайн мэдэхгүй бол суулгах талаар [энэ tutorial](https://hibbard.eu/install-ubuntu-virtual-box/)-ийг үзээрэй.

1. `~/.ssh/` directory рүү орж SSH түлхүүр байгаа эсэхийг шалгана уу. Байхгүй бол `ssh-keygen -a 100 -t ed25519` ашиглан үүсгээрэй. Passphrase болон `ssh-agent` ашиглавал дээр; дэлгэрэнгүй мэдээлэл [энд](https://www.ssh.com/ssh/agent) бий.

1. `.ssh/config` файлыг ингэж засварлана уу:

    ```bash
    Host vm
        User username_goes_here
        HostName ip_goes_here
        IdentityFile ~/.ssh/id_ed25519
        LocalForward 9999 localhost:8888
    ```

1. `ssh-copy-id vm` ажиллуулан өөрийн ssh нийтийн түлхүүрийг сервер рүү хуулна уу.

1. Өөрийн VM дотор `python -m http.server 8888` ажиллуулан вэб сервер эхлүүлнэ үү. Өөрийн локал машин дээр хөтчөөрөө `http://localhost:9999` хаягаар орон VM-ийн вэб сервер рүү хандана уу.

1. Өөрийн SSH серверийн тохиргоог `sudo vim /etc/ssh/sshd_config` командаар нээж `PasswordAuthentication` утгыг өөрчлөн нууц үгээр нэвтрэхийг хаана уу. Мөн `PermitRootLogin` утгыг өөрчлөн root хэрэглэгчээр нэвтрэхийг хаана уу. SSH үйлчилгээг `sudo service sshd restart` ажиллуулан дахин ачаалж, дахин нэвтэрч туршина уу.

1. (Асуулт) VM дотор [`mosh`](https://mosh.org/)-ийг суулгаж холболт үүсгэнэ үү. Дараа нь серверийн сүлжээний холболтыг салгана уу. Mosh холболтыг буцааж сэргээж чадаж байна уу?

1. (Асуулт) `ssh` командын `-N` болон `-f` flag-уудын үүргийг судалж, арын горимд портыг чиглүүлэх командыг олно уу.
