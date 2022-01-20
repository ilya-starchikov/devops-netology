# devops-netology
Hello, World!!!

В файле .gitignore:
```
- игнорируются все скрытые файлы ".terraform" с любой вложенностью 
- игнорируются все файлы заканчиващиеся на .tfstate или содержат в названии .tfstate. 
- игнорируется файл crash.log 
- игнорируются в корневом разделе файлы заканчиающиеся на .tfvars 
- игнорируются в корневом разделе файлы: override.tf, override.tf.json и заканчивающиеся на *_override.tf или *_override.tf.json 
- не игнорируется файл example_override.tf - не игнорируюися все файлы содержащие в названии tfplan (tfplan) 
- игнорируюися конфигурационные файлы .terraformrc и terraform.rc
```

# ДЗ «2.3. Ветвления в Git»

1. Создал каталог по Задание №1 с файлами merge.sh и rebase.sh
с содержимым:
```
#!/bin/bash
# display command line options

count=1
for param in "$*"; do
    echo "\$* Parameter #$count = $param"
    count=$(( $count + 1 ))
done
```

```
% git add -A
% git commit -m "prepare for merge and rebase"
% git push -u origin main
% git checkout -b git-merge
```
Содержимое файла merge.sh меняем на
```
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "\$@ Parameter #$count = $param"
    count=$(( $count + 1 ))
done
```

```
% git commit -am "merge: @ instead *"
% git push origin git-merge
```
Содержимое файла merge.sh меняем на
```
#!/bin/bash
# display command line options

count=1
while [[ -n "$1" ]]; do
    echo "Parameter #$count = $1"
    count=$(( $count + 1 ))
    shift
done
```
```
% git commit -am "merge: use shift"
% git push origin git-merge
% git checkout main
```
Содержимое файла rebase.sh меняем на
```
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "\$@ Parameter #$count = $param"
    count=$(( $count + 1 ))
done

echo "====="
```
```
% git commit -am "rebase: @ instead * and add echo"
% git push origin main
git switch -c git-rebase 52e734920e1d56e66b4dc830bf47778311886c07
```
Содержимое файла rebase.sh меняем на
```
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "Parameter: $param"
    count=$(( $count + 1 ))
done

echo "====="
```
```
% git commit -am "git-rebase 1"
% git push origin git-rebase
```
В файле rebase.sh содержимое меняем на
```
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "Next parameter: $param"
    count=$(( $count + 1 ))
done

echo "====="
```
```
% git commit -am "git-rebase 2"
% git push origin git-rebase
% git checkout main
$ git merge git-merge
% git checkout git-rebase
% git rebase -i main
```
В редакторе вторую строку приводим к виду:
```
fixup pick 3032c12 git-rebase 2
```
```
vim branching/rebase.sh
```
удалим метки, отдав предпочтение варианту
```
echo "\$@ Parameter #$count = $param"
```
```
% git add branching/rebase.sh"
% git rebase --continue
```
```
vim branching/rebase.sh
```
удалим метки, отдав предпочтение варианту
```
echo "Next parameter: $param"
```
```
% git add branching/rebase.sh"
% git rebase --continue
% git push -u origin git-rebase -f
% git checkout main
% git merge git-rebase
```

# ДЗ «2.4. Инструменты Git»

1. Найдите полный хеш и комментарий коммита, хеш которого начинается на ```aefea```.
```
% git show aefea
aefead2207ef7e2aa5dc81a34aedf0cad4c32545
```
2. Какому тегу соответствует коммит ```85024d3```?
```
% git show 85024d3
tag: v0.12.23
```
3. Сколько родителей у коммита ```b8d720```? Напишите их хеши.
```
% git log --pretty=%P -n 1 b8d720
2 родителя: 56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b
```
4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами ```v0.12.23``` и ```v0.12.24```.
```
% git log v0.12.23...v0.12.24
commit 33ff1c03bb960b332be3af2e333462dde88b279e (tag: v0.12.24)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Thu Mar 19 15:04:05 2020 +0000

    v0.12.24

commit b14b74c4939dcab573326f4e3ee2a62e23e12f89
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Tue Mar 10 08:59:20 2020 -0700

    [Website] vmc provider links

commit 3f235065b9347a758efadc92295b540ee0a5e26e
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Mar 19 10:39:31 2020 -0400

    Update CHANGELOG.md

commit 6ae64e247b332925b872447e9ce869657281c2bf
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Mar 19 10:20:10 2020 -0400

    registry: Fix panic when server is unreachable

    Non-HTTP errors previously resulted in a panic due to dereferencing the
    resp pointer while it was nil, as part of rendering the error message.
    This commit changes the error message formatting to cope with a nil
    response, and extends test coverage.

    Fixes #24384

commit 5c619ca1baf2e21a155fcdb4c264cc9e24a2a353
Author: Nick Fagerlund <nick.fagerlund@gmail.com>
Date:   Wed Mar 18 12:30:20 2020 -0700

    website: Remove links to the getting started guide's old location

    Since these links were in the soon-to-be-deprecated 0.11 language section, I
    think we can just remove them without needing to find an equivalent link.

commit 06275647e2b53d97d4f0a19a0fec11f6d69820b5
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Wed Mar 18 10:57:06 2020 -0400

    Update CHANGELOG.md

commit d5f9411f5108260320064349b757f55c09bc4b80
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Tue Mar 17 13:21:35 2020 -0400

    command: Fix bug when using terraform login on Windows

commit 4b6d06cc5dcb78af637bbb19c198faff37a066ed
Author: Pam Selle <pam@hashicorp.com>
Date:   Tue Mar 10 12:04:50 2020 -0400

    Update CHANGELOG.md

commit dd01a35078f040ca984cdd349f18d0b67e486c35
Author: Kristin Laemmert <mildwonkey@users.noreply.github.com>
Date:   Thu Mar 5 16:32:43 2020 -0500

    Update CHANGELOG.md

commit 225466bc3e5f35baa5d07197bbc079345b77525e
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Thu Mar 5 21:12:06 2020 +0000

    Cleanup after v0.12.23 release

```
5. Найдите коммит в котором была создана функция ```func providerSource```, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).
```
% git log -S"func providerSource"
```
Выбрал по наименьшей дате ```commit```
```
8c928e83589d90a031f811fae52a81be7153e82f
```
6. Найдите все коммиты в которых была изменена функция ```globalPluginDirs```.
```
% git log -S globalPluginDirs --oneline
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package
```
7. Кто автор функции ```synchronizedWriters```?
```
% git log -S synchronizedWriters
```
Выбрал по наименьшей дате ```commit```
```
Author: Martin Atkins <mart@degeneration.co.uk>
```

# ДЗ «3.1. Работа в терминале, лекция 1»
Пункты 1-4 являются подготовительными для выполнения заданий ниже.
5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
```
Оперативная память: 1024 МБ
Процессоры: 2
Видеопамять: 4 МБ
SATA Controller: 64 ГБ
Сеть: NAT
Из них, первые 4 по умолчанию.
```
6. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
```
Для это необходимо в Vagrantfile указать параметры
  v.memory = 1024
  v.cpus = 2
  или указать команды VM
  config.vm.provider "virtualbox" do |vb|
    # имя виртуальной машины
    vb.name = "ubuntu-2004"
    # объем оперативной памяти
    vb.memory = 
    # количество ядер процессора
    vb.cpus = 
```
8. Ознакомиться с разделами man bash, почитать о настройках самого bash:

    какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
    ```
   HISTFILESIZE line 627
    ```
   что делает директива ignoreboth в bash?
    ```
    Опция HISTCONTROL контролирует каким образом список команд сохраняется в истории.
    ignorespace — не сохранять строки начинающиеся с символа <пробел>
    ignoredups — не сохранять строки, совпадающие с последней выполненной командой
    ignoreboth — использовать обе опции ‘ignorespace’ и ‘ignoredups’
    ```
9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
```
скобки {} - вариант использования составных команд, указанных в man bash line 195
в этих скобках мы можем передавать список shell-команд и использовать регулярные выражения, которые выполнятся последовательно в текущем переменном окружении
так же этот список мы можем присвоить переменной 
```
10. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
```
touch {1..100000}
300000 файлов не получиться создать, мы получим ошибку "argument list too long" так как максимальная аргументов командной строки ограчены параметром ядра в 128 Кб
и определена в переменной ARG_MAX
```
11. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]
```
в примере опция "-d /tmp" возвращает True, если такой файл существует и является директорией (line 1381), иначе вернет False
[[выражение]] - квадратные скобки - еще один из вариантов составных команд, возвращает статус 0 или 1 в зависимости от оценки условного выражения выражения.
в нашем случае конструкция вернет значение - 0
```
12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:
```
bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
```
решение
```
mkdir /tmp/new_path_directory
cp /bin/bash /tmp/new_path_directory/
PATH=/tmp/new_path_directory/:$PATH

vagrant@vagrant:~$ type -a bash
bash is /tmp/new_path_directory/bash
bash is /usr/bin/bash
bash is /bin/bash
```
13. Чем отличается планирование команд с помощью batch и at?
```
at - выполняет команды в указанное время
batch - выполняет команды, когда это позволяют уровни загрузки системы; другими словами, когда средняя загрузка падает ниже 1,5 или значения, указанного при вызове atd. 
```
14. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.
```
vagrant halt
```

# ДЗ «3.2. Работа в терминале, лекция 2»
1. Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, 
если считаете что она могла бы быть другого типа.
```
Команда cd является встроенной в shell. Встроенная потому что она должна выполнятся в том же пересменном окружении что и shell. 
```
2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. 
```
grep <some_string> <some_file> -c
```
3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
```
systemd
```
4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?
```
ls -l /фзыф 2> /dev/pts/1 (1 - номер другой сессии терминала)
```
5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.
```
cat a.txt
a
b
c
d

cat < a.txt > b.txt

cat b.txt
a
b
c
d
```
6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?
```
Да,
root@vagrant:~# tty
/dev/pts/0
root@vagrant:~# echo abc > /dev/tty
abc
```
7. Выполните команду ```bash 5>&1```. К чему она приведет? Что будет, если вы выполните ```echo netology > /proc/$$/fd/5```? Почему так происходит?
```
bash 5>&1 - создаст fd 5 у PID=$$, и перенаправит вывод на stdout
echo netology > /proc/$$/fd/5 - выведет "netology", потому что &1
```
8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty?
```
command 5>&2 2>&1 1>&5 | command
```
9. Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?
```
cat /proc/$$/environ - выведет начальное переменное окружение процесса
Так же переменное окружение можно получить командами: env и printenv
```
10. Используя ```man```, опишите что доступно по адресам ```/proc/<PID>/cmdline```, ```/proc/<PID>/exe```.
```
/proc/<PID>/cmdline - это файл только для чтения, который содержит полную информацию о процессе из командной строки. Если процесс был заменен местами в дополнение к памяти или процесс является зомби-процессом, то этот файл не имеет содержимого. Файл заканчивается нулевым символом вместо символа новой строки.
/proc/<PID>/exe - этот файл представляет собой символическую ссылку, содержащую фактический путь к выполняемой команде 
```
11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью ```/proc/cpuinfo```
```
sse4_2
```
12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:
```
vagrant@netology1:~$ ssh localhost 'tty'
not a tty
```
Почитайте, почему так происходит, и как изменить поведение.
```
В случае выполения одиночной команды ssh на удаленном сервере не выделяется TTY
Исправить это можно опцией -t, принудительное выделение псевдотерминала: ssh -t localhost 'tty'
```
13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.
```
vi /etc/sysctl.d/10-ptrace.conf
kernel.yama.ptrace_scope = 0
reboot

top
CTRL+Z
bg
ps -a (копируем PID запущенного top)
screen -S test_top
reptyr PID
```
14. sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.
```
tee - читает из стандартного ввода и записывает как в стандартный вывод, в один или несколько файлов одновременно.
Так как shell работает в том переменном окружении пользователя в котором он запущен то перенаправить вывод в файл принадлежащий другому пользователю нельзя было.
Во втором случае команда tee запущена от root, поэтому были права на запись.
```
# ДЗ «3.3. Операционные системы, лекция 1»
1. Какой системный вызов делает команда cd?
```
chdir
```
2. Попробуйте использовать команду file на объекты разных типов на файловой системе.   
Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.
```
"/usr/share/misc/magic.mgc" - БД
```
3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).
```
cat /dev/null > /proc/PID/fd/number
```
4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?
```
зомби-процессы не занимают CPU, RAM но могут заблокировать IO программы и она не будет запускаться.
```
5. В iovisor BCC есть утилита ```opensnoop```:
```
root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
```
```
root@vagrant:~# opensnoop-bpfcc
PID    COMM               FD ERR PATH
634    irqbalance          6   0 /proc/interrupts
634    irqbalance          6   0 /proc/stat
634    irqbalance          6   0 /proc/irq/20/smp_affinity
634    irqbalance          6   0 /proc/irq/19/smp_affinity
634    irqbalance          6   0 /proc/irq/19/smp_affinity
634    irqbalance          6   0 /proc/irq/0/smp_affinity
634    irqbalance          6   0 /proc/irq/1/smp_affinity
634    irqbalance          6   0 /proc/irq/8/smp_affinity
634    irqbalance          6   0 /proc/irq/12/smp_affinity
```
6. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.
```
системный вызов uname

Part of the utsname information is also accessible via  
       /proc/sys/kernel/{ostype, hostname, osrelease, version,  
       domainname}. (c)
```
7. Чем отличается последовательность команд через ; и через && в bash? Например: 
```
oot@netology1:~# test -d /tmp/some_dir; echo Hi
Hi
root@netology1:~# test -d /tmp/some_dir && echo Hi
root@netology1:~#
```
```
'&&' - это логический оператор "И". Используется для объединения команд,  
таким образом, что следующая команда после "&&" выполнится в случае если  
предыдущая завершится с кодом возврата 0.    
с помощью ';' можно разделить команды и записать в одну строку
```
Есть ли смысл использовать в bash &&, если применить set -e?
```
не имеет, так как set -e завершит работу конвеера, если команда завершится с кодом возврата отличного от 0
```
8. Из каких опций состоит режим ```bash set -euxo pipefail``` и почему его хорошо было бы использовать в сценариях?
```
-e Немедленный выход, если команда завершается с ненулевым статусом.
-u Рассматривать неустановленные переменные как ошибку, с выводом в stderr
-x Печатать команды и их аргументы по мере их выполнения.
-o pipefail Возвращает результат работы всего конвеера по результат работы  
последней команды завершенной с ненулевым статусом, или ноль — если работа всех команд завершена успешно.

Использование этих оцпий посволяет понимать работу конвеера, и на каком этапе могут  
вознинуть ошибки
```
9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).
```
root@vagrant:~# ps -o stat
+ ps -o stat
STAT
S - Прерываемый сон (ожидание завершения события) 
S
R+ работающий или работающий (в очереди выполнения) ('+' находится в группе процессов переднего плана)
```