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
# ДЗ «3.4. Операционные системы, лекция 2»
1. На лекции мы познакомились с node_exporter. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:

    * поместите его в автозагрузку,
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.
```
root@vagrant# vim /etc/systemd/system/node_exporter.service

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
EnvironmentFile=/opt/node_exporter/node_exporter_options
ExecStart=/opt/node_exporter/node_exporter $OPTIONS

[Install]
WantedBy=multi-user.target

root@vagrant# vim /opt/node_exporter/node_exporter_options
OPTIONS="--web.listen-address=":9100""

root@vagrant# systemctl enable node_exporter
Created symlink /etc/systemd/system/multi-user.target.wants/node_exporter.service → /etc/systemd/system/node_exporter.service

root@vagrant# systemctl start node_exporter
root@vagrant# systemctl status node_exporter

node_exporter.service - Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2022-01-23 07:42:59 UTC; 6min ago
   Main PID: 1611 (node_exporter)
      Tasks: 4 (limit: 1107)
     Memory: 2.4M
     CGroup: /system.slice/node_exporter.service
             └─1611 /opt/node_exporter/node_exporter --web.listen-address=:9100
         
root@vagrant# systemctl stop node_exporter
root@vagrant# systemctl status node_exporter

● node_exporter.service - Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Sun 2022-01-23 07:50:30 UTC; 2s ago
    Process: 1611 ExecStart=/opt/node_exporter/node_exporter $OPTIONS (code=killed, signal=TERM)
   Main PID: 1611 (code=killed, signal=TERM)

Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.674Z caller=node_exporter.go:115 level=info collector=udp_queues
Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.674Z caller=node_exporter.go:115 level=info collector=uname
Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.674Z caller=node_exporter.go:115 level=info collector=vmstat
Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.674Z caller=node_exporter.go:115 level=info collector=xfs
Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.674Z caller=node_exporter.go:115 level=info collector=zfs
Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.675Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
Jan 23 07:42:59 vagrant node_exporter[1611]: ts=2022-01-23T07:42:59.675Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
Jan 23 07:50:30 vagrant systemd[1]: Stopping Node Exporter...
Jan 23 07:50:30 vagrant systemd[1]: node_exporter.service: Succeeded.
Jan 23 07:50:30 vagrant systemd[1]: Stopped Node Exporter.
```

2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
```
CPU:
   - node_cpu_seconds_total{*};
   - process_cpu_seconds_total;
Memory:
   - node_memory_MemAvailable_bytes;
   - node_memory_MemTotal_bytes;
   - node_vmstat_pgmajfault;
Disk:
   - node_disk_read_bytes_total{*};
   - node_disk_written_bytes_total{*};
   - node_filesystem_size_bytes{*};
   - node_filesystem_avail_bytes{*};
   - node_filesystem_files_free{*};
   - node_disk_read_time_seconds_total{*};
   - node_disk_write_time_seconds_total{*};
Network:
   - node_network_receive_bytes_total{*};
   - node_network_transmit_bytes_total{*};
   - node_network_receive_errs_total{*};
   - node_network_transmit_errs_total{*};
   - node_network_transmit_drop_total{*};
   - node_network_receive_drop_total{*};
```
3. Ознакомиться с netdata
```
vagrant@vagrant:~$ systemctl status netdata.service
● netdata.service - netdata - Real-time performance monitoring
     Loaded: loaded (/lib/systemd/system/netdata.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2022-01-23 08:31:51 UTC; 3min 48s ago
       Docs: man:netdata
             file:///usr/share/doc/netdata/html/index.html
             https://github.com/netdata/netdata
   Main PID: 638 (netdata)
      Tasks: 22 (limit: 1107)
     Memory: 65.6M
     CGroup: /system.slice/netdata.service
             ├─638 /usr/sbin/netdata -D
             ├─814 /usr/lib/netdata/plugins.d/nfacct.plugin 1
             ├─820 /usr/lib/netdata/plugins.d/apps.plugin 1
             └─822 bash /usr/lib/netdata/plugins.d/tc-qos-helper.sh 1

Jan 23 08:31:51 vagrant systemd[1]: Started netdata - Real-time performance monitoring.
Jan 23 08:31:51 vagrant netdata[638]: SIGNAL: Not enabling reaper
Jan 23 08:31:51 vagrant netdata[638]: 2022-01-23 08:31:51: netdata INFO  : MAIN : SIGNAL: Not enabling reaper

vagrant@vagrant:~$ ss -tulpn | grep 19999
tcp   LISTEN  0       4096            0.0.0.0:19999        0.0.0.0:*
```

4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
```
Да.
root@vagrant:~# dmesg -T | grep virt
[Sun Jan 23 08:31:41 2022] CPU MTRRs all blank - virtualized system.
[Sun Jan 23 08:31:41 2022] Booting paravirtualized kernel on KVM
[Sun Jan 23 08:31:46 2022] systemd[1]: Detected virtualization oracle.
```
5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?
```
root@vagrant:~# sysctl -n fs.nr_open
1048576
Лимит количества открытых дескрипторов

ulimit -aH    
open files                      (-n) 1048576
```
6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.
```
root@vagrant:~# ps -e |grep sleep
  1723 pts/2    00:00:00 sleep
root@vagrant:~# nsenter --target 1723 --pid --mount
root@vagrant:/# ps
    PID TTY          TIME CMD
      2 pts/0    00:00:00 bash
     11 pts/0    00:00:00 ps
```
7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?
```
:(){ :|:& };: - так называемая fork бомба, функция, которая параллельно пускает два своих экземпляра. Каждый пускает ещё по два и т.д. 
При отсутствии лимита на число процессов машина быстро исчерпывает физическую память и уходит в своп.
Сработал oomkiller
```

# ДЗ «3.5. Файловые системы»
1. Узнайте о sparse (разряженных) файлах.
```
Узнал
```
2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?
```
Нет, так как права доступа выдаются на inode.   
```
3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:
   (не стал копировать весь текст)
```
Сделал.
vagrant@vagrant:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop /snap/core18/2128
loop1                       7:1    0 32.3M  1 loop /snap/snapd/12704
loop2                       7:2    0 70.3M  1 loop /snap/lxd/21029
loop3                       7:3    0 55.5M  1 loop /snap/core18/2284
loop4                       7:4    0 43.4M  1 loop /snap/snapd/14549
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   63G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm  /
sdb                         8:16   0  2.5G  0 disk
sdc                         8:32   0  2.5G  0 disk
```
4. Используя ```fdisk```, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
```
root@vagrant:~# fdisk -l /dev/sdb
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x610becf6

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdb1          2048 4196351 4194304    2G 83 Linux
/dev/sdb2       4196352 5242879 1046528  511M 83 Linux
```
5. Используя ```sfdisk```, перенесите данную таблицу разделов на второй диск.
```
root@vagrant:~# sfdisk -d /dev/sdb | sfdisk --force /dev/sdc
Checking that no-one is using this disk right now ... OK
```
6. Соберите mdadm RAID1 на паре разделов 2 Гб.
```
root@vagrant:~# mdadm --create --verbose /dev/md0 --level 1 -n 2 /dev/sdb1 /dev/sdc1
```
7. Соберите mdadm RAID0 на второй паре маленьких разделов.
```
mdadm --create --verbose /dev/md1 --level 0 -n 2 /dev/sdb2 /dev/sdc2
```
```
echo "DEVICE partitions" >> /etc/mdadm/mdadm.conf
mdadm --detail --scan --verbose | awk '/ARRAY/ {print}' >> /etc/mdadm/mdadm.conf
```
8. Создайте 2 независимых PV на получившихся md-устройствах.
```
root@vagrant:~# pvcreate /dev/md0
root@vagrant:~# pvcreate /dev/md1
```
9. Создайте общую volume-group на этих двух PV.
```
vgcreate vg0 /dev/md0 /dev/md1
```
10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.
```
root@vagrant:~# lvcreate -n lv-test -L100M vg0 /dev/md1
```
11. Создайте mkfs.ext4 ФС на получившемся LV.
```
root@vagrant:~# mkfs.ext4 /dev/vg0/lv-test

root@vagrant:~# blkid | grep vg0
/dev/mapper/vg0-lv--test: UUID="e7dd4eee-52ad-41ca-82df-158dd5336627" TYPE="ext4"
```
12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.
```
root@vagrant:~# mkdir /tmp/new

root@vagrant:~# tail -n1 /etc/fstab
UUID=e7dd4eee-52ad-41ca-82df-158dd5336627 /tmp/new ext4 defaults 0 1

root@vagrant:~# mount -a
```
13. Поместите туда тестовый файл, например ```wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz```.
```
root@vagrant:~# ls /tmp/new
lost+found  test.gz
```
14. Прикрепите вывод lsblk
```
root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
loop1                       7:1    0 32.3M  1 loop  /snap/snapd/12704
loop2                       7:2    0 70.3M  1 loop  /snap/lxd/21029
loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
loop4                       7:4    0 43.4M  1 loop  /snap/snapd/14549
loop5                       7:5    0 61.9M  1 loop  /snap/core20/1270
loop6                       7:6    0 67.2M  1 loop  /snap/lxd/21835
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part  /boot
└─sda3                      8:3    0   63G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─vg0-lv--test        253:1    0  100M  0 lvm   /tmp/new
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─vg0-lv--test        253:1    0  100M  0 lvm   /tmp/new
```
15. Протестируйте целостность файла:
```
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
```
```
Такой же вывод - 0
```
16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
```
root@vagrant:~# pvmove /dev/md1
  /dev/md1: Moved: 32.00%
  /dev/md1: Moved: 100.00%
root@vagrant:~#
root@vagrant:~#
root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
loop1                       7:1    0 32.3M  1 loop  /snap/snapd/12704
loop2                       7:2    0 70.3M  1 loop  /snap/lxd/21029
loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
loop4                       7:4    0 43.4M  1 loop  /snap/snapd/14549
loop5                       7:5    0 61.9M  1 loop  /snap/core20/1270
loop6                       7:6    0 67.2M  1 loop  /snap/lxd/21835
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part  /boot
└─sda3                      8:3    0   63G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
│   └─vg0-lv--test        253:1    0  100M  0 lvm   /tmp/new
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
│   └─vg0-lv--test        253:1    0  100M  0 lvm   /tmp/new
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
```
17. Сделайте ```--fail``` на устройство в вашем RAID1 md.
```
root@vagrant:~# mdadm /dev/md0 --fail /dev/sdb1
mdadm: set /dev/sdb1 faulty in /dev/md0
```
18. Подтвердите выводом ```dmesg```, что RAID1 работает в деградированном состоянии.
```
root@vagrant:~# dmesg -T | grep md0
[Tue Jan 25 11:40:32 2022] md/raid1:md0: not clean -- starting background reconstruction
[Tue Jan 25 11:40:32 2022] md/raid1:md0: active with 2 out of 2 mirrors
[Tue Jan 25 11:40:32 2022] md0: detected capacity change from 0 to 2144337920
[Tue Jan 25 11:40:32 2022] md: resync of RAID array md0
[Tue Jan 25 11:40:42 2022] md: md0: resync done.
[Tue Jan 25 12:16:55 2022] md/raid1:md0: Disk failure on sdb1, disabling device.
                           md/raid1:md0: Operation continuing on 1 devices.
```
19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:
```
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
```
```
Такой же вывод - 0
```
20. Погасите тестовый хост, ```vagrant destroy```.
```
done!
```

# ДЗ «3.6. Компьютерные сети, лекция 1»
1. Работа c HTTP через телнет. Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
    отправьте HTTP запрос
```
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
В ответе укажите полученный HTTP код, что он означает?
```
HTTP/1.1 301 Moved Permanently
location: https://stackoverflow.com/questions

Выволнен редирект в указанный location
```
2. Повторите задание 1 в браузере, используя консоль разработчика F12.

* откройте вкладку Network
* отправьте запрос http://stackoverflow.com
* найдите первый ответ HTTP сервера, откройте вкладку Headers
* укажите в ответе полученный HTTP код.
* проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
* приложите скриншот консоли браузера в ответ.
```
Status Code: 301 Moved Permanently
Время загрузки страницы - 393мс
Дольшего всего выполнялся js - 891.05мс

![dev-tools](https://github.com/ilya-starchikov/devops-netology/blob/main/dev-tools.png)
```
3. Какой IP адрес у вас в интернете?
```
vagrant@vagrant:~$ curl ifconfig.me
185.57.29.48
```
4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois
```
root@vagrant:~# whois 185.57.29.48

netname:        OCTOPUSNET-NET
origin:         AS44724
```
5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute
```
root@vagrant:~# traceroute -n 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  10.0.2.2  0.192 ms  0.156 ms  0.142 ms
 2  192.168.78.1  1.904 ms  1.889 ms  4.551 ms
 3  10.81.160.1  4.533 ms  4.513 ms  4.446 ms
 4  10.254.186.1  4.522 ms  4.375 ms  4.490 ms
 5  10.254.191.19  5.633 ms  5.360 ms  7.171 ms
 6  95.167.218.81  7.145 ms  7.217 ms  7.535 ms
 7  87.226.183.89  111.868 ms 87.226.181.89  110.680 ms  111.188 ms
 8  5.143.253.105  112.068 ms 5.143.253.245  112.962 ms 74.125.51.172  113.531 ms
 9  108.170.250.83  121.885 ms 108.170.250.34  112.531 ms 108.170.250.66  113.477 ms
10  * * *
11  72.14.238.168  127.168 ms 216.239.43.20  127.457 ms 108.170.235.64  123.610 ms
12  209.85.251.63  134.013 ms 142.250.208.25  130.666 ms 142.250.209.25  135.138 ms
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * 8.8.8.8  135.550 ms
```
6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?
```
mtr -n -r 8.8.8.8 > mtr-report & cat mtr-report
Start: 2022-02-09T11:59:57+0000
HOST: vagrant                     Loss%   Snt   Last   Avg  Best  Wrst StDev
  1.|-- 10.0.2.2                   0.0%    10    0.3   0.2   0.2   0.4   0.1
  2.|-- 192.168.78.1               0.0%    10    1.5   2.5   1.2   7.9   2.0
  3.|-- 10.81.160.1                0.0%    10    4.7   2.5   1.6   4.7   0.9
  4.|-- 10.254.186.1               0.0%    10    4.8   4.1   2.2   9.2   2.2
  5.|-- 10.254.191.19              0.0%    10    3.9   7.9   2.7  24.0   7.6
  6.|-- 95.167.218.81             10.0%    10    5.9   6.8   4.4  10.3   1.9
  7.|-- 87.226.181.89              0.0%    10  121.3 117.5 109.9 130.4   6.6
  8.|-- 5.143.253.105             10.0%    10  123.3 114.1 110.5 123.3   4.3
  9.|-- 108.170.250.34             0.0%    10  113.3 115.3 111.4 127.7   5.3
 10.|-- 142.251.49.24             40.0%    10  135.1 133.6 131.1 135.8   2.0
 11.|-- 108.170.235.64             0.0%    10  125.9 126.7 123.5 133.6   3.2
 12.|-- 142.250.209.35             0.0%    10  131.0 134.1 131.0 141.3   3.1
 13.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
 14.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
 15.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
 16.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
 17.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
 18.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
 19.|-- 8.8.8.8                   90.0%    10  131.8 131.8 131.8 131.8   0.0
 
 Самый высокий delay на 10 шаге, как и высокий процент потерь
```
7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig
```
dig dns.google

;; ANSWER SECTION:
dns.google.		111	IN	A	8.8.8.8
dns.google.		111	IN	A	8.8.4.4
```
8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig
```
dig -x 8.8.8.8

;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.	6318	IN	PTR	dns.google.


dig -x 8.8.4.4

;; ANSWER SECTION:
4.4.8.8.in-addr.arpa.	34411	IN	PTR	dns.google.
```

# ДЗ «3.7. Компьютерные сети, лекция 2»
1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?
```
Windows:
   - ipconfig
Linux:
   - ifconfig
   - ip -c a
   - hostname -I | awk '{print $1}'
```
2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?
```
Для распозования используется протокол lldp и ip.
Утилиты ставиться из пакета lldpd и net-tools

Команды: 
   - lldpcli show neighbors
   - ip neigh show
```
3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.
```
Для разделения на несколько виртуальных сетей используется технология vlan
Ипользуется одноименный пакет vlan с командой - vconfig, либо можно пользоваться 
стандартными утилитами, где vlan у интерфейса указывается через точку

sudo vconfig add eth1 10
sudo ip link add link eth1 name eth1.10 type vlan id 10

sudo su -c 'echo "8021q" >> /etc/modules'
vim /etc/network/interfaces
auto eth1.10
iface eth1.10 inet static
    address 10.0.0.1
    netmask 255.255.255.0
    vlan-raw-device eth1
```
4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.
```
Агрегаци осуществляется через - bonding

Опции балансировки:
    - mode=0 (balance-rr)
    - mode=1 (active-backup)
    - mode=2 (balance-xor)
    - mode=3 (broadcast)
    - mode=4 (802.3ad)
    - mode=5 (balance-tlb)
    - mode=6 (balance-alb)

Config:

vim /etc/network/interfaces 
auto bond0
iface bond0 inet static
	address 10.0.0.5
	gateway 10.0.0.1
	broadcast 10.0.0.255
	netmask 255.255.255.0
	up /sbin/ifenslave bond0 eth1 eth2
	down /sbin/ifenslave -d bond0 eth0 eth1
```
5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.
```
/29 - 6 IP адресов
из /24 подсети можно получить 32 подсети /29

Пример - 10.10.10.8/29
```
6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.
```
100.64x.0.0/26
```
7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?
```
arp #таблица arp
ip -s -s neigh flush all #очистить кэш полностью
arp -d 192.168.1.1 #удалить конкретный адрес
```
# ДЗ "3.8. Компьютерные сети, лекция 3"
1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
```
root@vagrant:~# curl ifconfig.me
185.57.29.48

route-views>show ip route | i 185.57.29.0
B        185.57.29.0/24 [20/0] via 64.71.137.241, 1d09h

route-views>show bgp | i 185.57.29.0
N*   185.57.29.0/24   217.192.89.50                          0 3303 12389 44724 i
```
2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.
```
modprobe -v dummy
ip link add dummy0 type dummy
ip link set dev dummy0 up
ip addr add 192.168.1.57/24 dev dummy0
ip route add 172.16.36.27/32 dev dummy0

root@vagrant:~# ip -c a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:b1:28:5d brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 84302sec preferred_lft 84302sec
    inet6 fe80::a00:27ff:feb1:285d/64 scope link
       valid_lft forever preferred_lft forever
3: dummy0: <BROADCAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether 52:84:74:f8:ce:3c brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.57/24 scope global dummy0
       valid_lft forever preferred_lft forever
    inet6 fe80::5084:74ff:fef8:ce3c/64 scope link
       valid_lft forever preferred_lft forever
       

root@vagrant:~# ip r
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
172.16.36.27 dev dummy0 scope link
192.168.1.0/24 dev dummy0 proto kernel scope link src 192.168.1.57
```
3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.
```
root@vagrant:~# ss -tlpn
State               Recv-Q              Send-Q                           Local Address:Port                           Peer Address:Port             Process
LISTEN              0                   4096                             127.0.0.53%lo:53                                  0.0.0.0:*                 users:(("systemd-resolve",pid=678,fd=13))
LISTEN              0                   128                                    0.0.0.0:22                                  0.0.0.0:*                 users:(("sshd",pid=749,fd=3))
LISTEN              0                   128                                       [::]:22                                     [::]:*                 users:(("sshd",pid=749,fd=4))
root@vagrant:~#
```
4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?
```
root@vagrant:~# ss -ulpn
State               Recv-Q              Send-Q                            Local Address:Port                           Peer Address:Port             Process
UNCONN              0                   0                                 127.0.0.53%lo:53                                  0.0.0.0:*                 users:(("systemd-resolve",pid=678,fd=12))
UNCONN              0                   0                                10.0.2.15%eth0:68                                  0.0.0.0:*                 users:(("systemd-network",pid=676,fd=19))
root@vagrant:~#
```
5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.
```
![network](https://github.com/ilya-starchikov/devops-netology/blob/main/network.drawio.png)
```
# ДЗ 3.9. Элементы безопасности информационных систем

1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.
```
![Bitwarden](https://github.com/ilya-starchikov/devops-netology/blob/main/bitwarden.png)
```
2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
```
Сделал, это просто ужасное приложение. 
```
3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
```
Генерируем самоподписанный сертификат:
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
(везде жал просто Enter, для создания apache-selfsigned.key ввел passphrase)

Пример конфига apache:
vim /etc/apache2/sites-available/default-ssl.conf

<IfModule mod_ssl.c>
        <VirtualHost _default_:443>
                ServerAdmin superadmin@test-srv.com
                ServerName test-srv.com

                DocumentRoot /var/www/html

                ErrorLog ${APACHE_LOG_DIR}/error.log
                CustomLog ${APACHE_LOG_DIR}/access.log combined

                SSLEngine on

                SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
                SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>

        </VirtualHost>
</IfModule>

a2enmod ssl
a2enmod headers
a2ensite default-ssl
a2enconf ssl-params

vim /etc/apache2/sites-enabled/000-default
<VirtualHost *:80>
        . . .

        Redirect permanent "/" "https://test-srv.com/"

        . . .
</VirtualHost>

systemctl restart apache2

в /etc/hosts добавлен test-srv.com

root@vagrant:~# curl -s -I -XGET http://test-srv.com
HTTP/1.1 302 Found
Date: Thu, 03 Mar 2022 12:37:49 GMT
Server: Apache/2.4.41 (Ubuntu)
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Location: https://test-srv.com/
Content-Length: 277
Content-Type: text/html; charset=iso-8859-1
```
4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
```
root@vagrant:~/testssl.sh# ./testssl.sh -U --sneaky https://www.proplay.ru/

rDNS (212.42.38.206):   www.proplay.ru.
 Service detected:       HTTP


 Testing vulnerabilities

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), timed out
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                           Can be ignored for static pages or if no secrets in the page
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              Downgrade attack prevention supported (OK)
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                           https://censys.io/ipv4?q=0D95A3947E974E8D34FC0062463EC9AC9A2E73F2AC1E4049C91C3C9C7D253D48 could help you to find out
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     TLS1: ECDHE-RSA-AES256-SHA AES256-SHA CAMELLIA256-SHA ECDHE-RSA-AES128-SHA AES128-SHA CAMELLIA128-SHA
                                           VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK) - CAMELLIA or ECDHE_RSA GCM ciphers found
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


 Done 2022-02-20 13:01:14 [  96s] -->> 212.42.38.206:443 (www.proplay.ru) <<--
```
5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
```
vm1: 
ssh-keygen -t rsa
cd ./ssh
ssh-copy-id -i id_rsa.pub vagrant@10.20.2.5

vm2:
vim /etc/ssh/sshd_config
PermitRootLogin no 
PasswordAuthentication no 

systemctl restart ssh.service
ufw allow ssh
```
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.
```
mv ~/.ssh/id_dsa ~/.ssh/id_dsa_bla_bla
vim ~/.ssh/config
Host *
  IdentitiesOnly yes
Host test-vm2
  Hostname 10.20.2.5
  User vagrant
  IdentityFile ~/.ssh/id_dsa_bla_bla
```
7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
```
tcpdump -ni lo0 -c 100 icmp -w test.dump

![wireshark](https://github.com/ilya-starchikov/devops-netology/blob/main/wireshark.png)
```
# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

## Обязательная задача 1

Есть скрипт:
```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- |----------|--|
| `c`  | a+b      | по умолчанию значение переменной - строка, вывели строку содержащую символы|
| `d`  | 1+2      | по умолчанию значение переменной - строка, вывели строку содержащую символы, значений этих переменных, подстановка выполнилась через $|
| `e`  | 3        | вывели строку, результатом которой стала итог математической операции за счет подстановка значений ко всему выражению |


## Обязательная задача 2
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

### Ваш скрипт:
```bash
while ((1==1))
do
        curl https://localhost:4757
        if (($? != 0))
        then
                echo `date` >> curl.log
        fi
done
```

## Обязательная задача 3
Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
for (( i = 1; i <= 5; i++ ))
do
	echo "Start $i:"
for host in "192.168.0.1" "173.194.222.113" "87.250.250.242"
do
	echo "Try $host"
	timeout 5 nc -vz $host 80 2>> curl.log
	if (($? != 0))
	then
		echo "Can't connect to $host and port 80" >> curl.log
	fi
done
done
```

## Обязательная задача 4
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
for (( i = 1; i <= 5; i++ ))
do
	echo "Start $i:"
for host in "192.168.0.1" "173.194.222.113" "87.250.250.242"
do
	echo "Try $host"
	timeout 5 nc -vz $host 80 2>> curl.log
	if (($? != 0))
	then
		echo "$host" >> error.log
		exit 1
	fi
done
done
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: \[04-script-01-bash\] сломал хук.

### Ваш скрипт:
```bash
???
```

### Курсовая работа по итогам модуля "DevOps и системное администрирование"

Установите ufw и разрешите к этой машине сессии на порты 22 и 443, при этом трафик на интерфейсе localhost (lo) должен ходить свободно на все порты.
```commandline
ufw allow 22
ufw allow 443
ufw allow in on lo
ufw allow out on lo
ufw enable

ufw status
Status: active

To                         Action      From
--                         ------      ----
22                         ALLOW       Anywhere
443                        ALLOW       Anywhere
Anywhere on lo             ALLOW       Anywhere
Anywhere                   ALLOW OUT   Anywhere on lo
```

Установите hashicorp vault
```commandline
wget https://hashicorp-releases.website.yandexcloud.net/vault/1.9.3/vault_1.9.3_linux_amd64.zip
apt install unzip jq
unzip vault_1.9.3_linux_amd64.zip
mv vault /usr/local/bin/
```

Cоздайте центр сертификации по инструкции и выпустите сертификат для использования его в настройке веб-сервера nginx (срок жизни сертификата - месяц).
```commandline
Start Vault:
vault server -dev -dev-root-token-id root
export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root

Step 1: Generate root CA:
vault secrets enable pki
vault secrets tune -max-lease-ttl=87600h pki
vault write -field=certificate pki/root/generate/internal \
     common_name="example.com" \
     ttl=87600h > CA_cert.crt
vault write pki/config/urls \
     issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
     crl_distribution_points="$VAULT_ADDR/v1/pki/crl"

Step 2: Generate intermediate CA:
vault secrets enable -path=pki_int pki
vault secrets tune -max-lease-ttl=43800h pki_int
vault write -format=json pki_int/intermediate/generate/internal \
     common_name="example.com Intermediate Authority" \
     | jq -r '.data.csr' > pki_intermediate.csr
vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr \
     format=pem_bundle ttl="43800h" \
     | jq -r '.data.certificate' > intermediate.cert.pem
vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem

Step 3: Create a role:
vault write pki_int/roles/example-dot-com \
     allowed_domains="example.com" \
     allow_subdomains=true \
     max_ttl="750h"

Step 4: Request certificates:
vault write -format=json pki_int/issue/example-dot-com common_name="test.example.com" ttl="750h" > /root/test.example.com.json
```

Установите корневой сертификат созданного центра сертификации в доверенные в хостовой системе.
```commandline
scp -P 2222 vagrant@127.0.0.1:/home/vagrant/CA_cert.crt .
Добавил этот сертифакат в MAC OS в системную связку ключей
![system](https://github.com/ilya-starchikov/devops-netology/blob/main/cert.png )
```

Установите nginx
```commandline
vim /etc/apt/sources.list.d/nginx.list
```
```commandline
deb https://nginx.org/packages/ubuntu/ focal nginx
deb-src https://nginx.org/packages/ubuntu/ focal nginx
```
```commandline
apt update
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $key (key на который ругается)
apt update
apt install nginx

nginx -version
nginx version: nginx/1.20.2
```

По инструкции (ссылка) настройте nginx на https, используя ранее подготовленный сертификат:
    - можно использовать стандартную стартовую страницу nginx для демонстрации работы сервера;
    - можно использовать и другой html файл, сделанный вами;
```commandline
mkdir /etc/nginx/cert/

cat /root/test.example.com.json | jq -r .data.certificate > /etc/nginx/cert/test.example.com.crt
cat /root/test.example.com.json | jq -r .data.issuing_ca >> /etc/nginx/cert/test.example.com.crt
cat /root/test.example.com.json | jq -r .data.private_key > /etc/nginx/cert/test.example.com.key
```
```commandline
vim /etc/nginx/conf.d/default.conf
```
```commandline
server {
    listen       443 ssl;
    server_name  test.example.com;

    access_log  /var/log/nginx/host.access.log  main;
    ssl_certificate	/etc/nginx/cert/test.example.com.crt;
    ssl_certificate_key /etc/nginx/cert/test.example.com.key;
    ...
```
```commandline    
nginx -t
systemctl restart nginx
```

Откройте в браузере на хосте https адрес страницы, которую обслуживает сервер nginx.
```commandline
![site](https://github.com/ilya-starchikov/devops-netology/blob/main/site.png)
```

Создайте скрипт, который будет генерировать новый сертификат в vault:
    - генерируем новый сертификат так, чтобы не переписывать конфиг nginx;
    - перезапускаем nginx для применения нового сертификата.

```commandline
vim /root/generate_cert.sh
```
```commandline
#/usr/bin/env bash

export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root

vault write -format=json pki_int/issue/example-dot-com common_name="test.example.com" ttl="750h" > /root/test.example.com.json
cat /root/test.example.com.json | jq -r .data.certificate > /etc/nginx/cert/test.example.com.crt
cat /root/test.example.com.json | jq -r .data.issuing_ca >> /etc/nginx/cert/test.example.com.crt
cat /root/test.example.com.json | jq -r .data.private_key > /etc/nginx/cert/test.example.com.key

systemctl restart nginx

rm /root/test.example.com.json
```
```commandline
chmod +x /root/generate_cert.sh
```

Поместите скрипт в crontab, чтобы сертификат обновлялся какого-то числа каждого месяца в удобное для вас время.
```commandline
echo "* 0 1 * * root /root/generate_cert.sh" >> /etc/crontab
```
log
```commandline
journalctl -u cron.service
```
```commandline
Apr 16 04:41:01 vagrant CRON[1878]: pam_unix(cron:session): session opened for user root by (uid=0)
Apr 16 04:41:01 vagrant CRON[1879]: (root) CMD (/root/generate_cert.sh >> /var/log/cron.log 2>&1)
Apr 16 04:41:02 vagrant CRON[1878]: pam_unix(cron:session): session closed for user root
Apr 16 04:42:01 vagrant CRON[1906]: pam_unix(cron:session): session opened for user root by (uid=0)
Apr 16 04:42:01 vagrant CRON[1907]: (root) CMD (/root/generate_cert.sh >> /var/log/cron.log 2>&1)
Apr 16 04:42:01 vagrant CRON[1906]: pam_unix(cron:session): session closed for user root
Apr 16 04:43:01 vagrant CRON[1935]: pam_unix(cron:session): session opened for user root by (uid=0)
Apr 16 04:43:01 vagrant CRON[1936]: (root) CMD (/root/generate_cert.sh >> /var/log/cron.log 2>&1)
Apr 16 04:43:01 vagrant CRON[1935]: pam_unix(cron:session): session closed for user root
Apr 16 04:44:01 vagrant CRON[1992]: pam_unix(cron:session): session opened for user root by (uid=0)
Apr 16 04:44:01 vagrant CRON[1993]: (root) CMD (/root/generate_cert.sh >> /var/log/cron.log 2>&1)
Apr 16 04:44:01 vagrant CRON[1992]: pam_unix(cron:session): session closed for user root
Apr 16 04:45:01 vagrant CRON[2044]: pam_unix(cron:session): session opened for user root by (uid=0)
Apr 16 04:45:01 vagrant CRON[2045]: (root) CMD (/root/generate_cert.sh >> /var/log/cron.log 2>&1)
Apr 16 04:45:02 vagrant CRON[2044]: pam_unix(cron:session): session closed for user root
Apr 16 04:46:01 vagrant CRON[2076]: pam_unix(cron:session): session opened for user root by (uid=0)
Apr 16 04:46:01 vagrant CRON[2077]: (root) CMD (/root/generate_cert.sh >> /var/log/cron.log 2>&1)
Apr 16 04:46:01 vagrant CRON[2076]: pam_unix(cron:session): session closed for user root
```
```commandline
root@vagrant:/var/log# stat /etc/nginx/cert/test.example.com.key
  File: /etc/nginx/cert/test.example.com.key
  Size: 1675      	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 669900      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2022-04-16 04:45:02.079701431 +0000
Modify: 2022-04-16 04:45:02.067695430 +0000
Change: 2022-04-16 04:45:02.067695430 +0000
 Birth: -
root@vagrant:/var/log# stat /etc/nginx/cert/test.example.com.crt
  File: /etc/nginx/cert/test.example.com.crt
  Size: 2567      	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 668541      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2022-04-16 04:45:02.079701431 +0000
Modify: 2022-04-16 04:45:02.035679429 +0000
Change: 2022-04-16 04:45:02.035679429 +0000
 Birth: -
```

# ДЗ "4.2. Использование Python для решения типовых DevOps задач"

##Обязательная задача 1
Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```
### Вопросы:
| Вопрос  | Ответ                                                                                  |
| ------------- |----------------------------------------------------------------------------------------|
| Какое значение будет присвоено переменной `c`?  | Никакого. Будет ошибка - TypeError: unsupported operand type(s) for +: 'int' and 'str' |
| Как получить для переменной `c` значение 12?  | c = str(a) + b                                                                         |
| Как получить для переменной `c` значение 3?  | c = a + int(b)                                                                         |
## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
Поставил комменты в лишние строки: 
    - лишняя переменная is_change, 
    - выполняется прерывание (break) при первом вхождении
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/devops3-netology", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
#        break
```

### Вывод скрипта при запуске при тестировании:
```
./check_git.py
   README.md
   has_been_moved.txt
```
## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os
import sys # импортурем модуль чтобы получить доступ к аргументам

path = sys.argv[1] # берем первый аргумент 
bash_command = ["cd " + path, "git status"] # передаем его cd
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
#        break
```

### Вывод скрипта при запуске при тестировании:
```
./check_git.py ~/Documents/netology/devops/git/devops-netology/devops-netology
   README.md
   has_been_moved.txt
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import socket
import time

hosts = {'drive.google.com': None, 'mail.google.com': None, 'google.com': None}

while True:
  for host in hosts:
    time.sleep(2)
    ip = socket.gethostbyname(host)
    print(host + ' - ' + ip)
    if ip != hosts[host]:
      print('[ERROR] ' + host + ' IP mistmatch: ' + str(hosts[host]) + ' ' + ip)
      hosts[host] = ip
```

### Вывод скрипта при запуске при тестировании:
```
./check_ip.py
drive.google.com - 64.233.165.194
[ERROR] drive.google.com IP mistmatch: None 64.233.165.194
mail.google.com - 74.125.205.18
[ERROR] mail.google.com IP mistmatch: None 74.125.205.18
google.com - 173.194.222.101
[ERROR] google.com IP mistmatch: None 173.194.222.101
drive.google.com - 64.233.165.194
mail.google.com - 74.125.205.18
google.com - 173.194.222.101
drive.google.com - 64.233.165.194
```