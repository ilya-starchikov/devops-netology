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
git push -u origin git-rebase -f
% git checkout main
% git merge git-rebase
```
