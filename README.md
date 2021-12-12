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
aefead2207ef7e2aa5dc81a34aedf0cad4c32545
```
2. Какому тегу соответствует коммит ```85024d3```?
```
tag: v0.12.23
```
3. Сколько родителей у коммита ```b8d720```? Напишите их хеши.
```
2 родителя: 56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b
```
4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами ```v0.12.23``` и ```v0.12.24```.
```
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
5af1e6234ab6da412fb8637393c5a17a1b293663
```
6. Найдите все коммиты в которых была изменена функция ```globalPluginDirs```.
```
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package
```
7. Кто автор функции ```synchronizedWriters```?
```
Author: Martin Atkins <mart@degeneration.co.uk>
```