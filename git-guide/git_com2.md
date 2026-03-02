# First step

---

### Управления ветками

** git branch ** - Команда используется для управления ветками в репозиторит Git. 
с ее помощью можно создавать, удалять и перечислять ветки. Ветки в Git позволяют 
вам изолировать работу на различных функциях, исправлениях или версиях проекта

**git branch <названия ветки> ** - Создания ветки с вольным названием 

## Основные команды 

1) git branch -d <названия ветки> - удаляет указанною ветку

2) git branch -D <названия ветки> - принудительно удаляет ветку, даже если она не была объединена

3) git branch --move <старое названия> <новое названия> - изменить названия ветки 
или коротко git branch -m <старое названия> <новое названия>

4) git branch -a(or --all) - показывает все ветки: локальные и удаленные

5) git branch -r(or --remotes) - показывает только удаленные ветки

6) git branch <name> <commit> - создает ветку на укащанном коммите или теге

---

### Переключение между ветками

**git switch** - Команда используется для переключения между ветками

``` git switch <названия ветки> ``` 

Также могли создать ветку при помощи git switch  и сразу переключиться на нее при помощи флага -с

git switch -c develop

```
git switch -c(--create) <Имя новой ветки> - создает новую ветку и переключается на нее 

git switch -t(--track) - устанавливает связь с отслеживаемой удаленной веткой
```

---

## Временное хранилище

```
git stash
```

Команда используется для временного сохранения (stash) изменений в рабочем каталоге, которые еще 
не готовы для коммита. Это полезно, когда нужно переключиться на другую ветку или работу, но текущие изменения не 
должны быть потеряны. С помощью git stash изменения сохраняются в специальное место, откуда их можно восстановить 
позже.

1) Для начала создадим удаленную ветку origin/feature/111_add_print. Для этого переключимся на новую ветку 
feature/111_add_print, добавим немного кода в index.js и в index.css, сделаем коммит и запушим:


```
git switch -c feature/111_add_print

echo "function Print(){}" >> index.js
echo "p { text-align: center; }" >> index.css

git add .
git commit -m "Add print function"
git push

```

2) "Перелопатим" код (добавим изменения в файл index.js)

```
echo "const y = true;" >> index.js
echo "console.log(y)" >> index.js
```

3) Так как нам нужно исправлять код рабочий — наследоваться от ветки develop, а не от 
feature/111_add_print, то нам для начала необходимо переключиться на develop и от него создать ветку 
hotfix/change_clg:

```
git switch develop

error: Your local changes to the following files would be overwritten by checkout:
        index.js
Please commit your changes or stash them before you switch branches.
Aborting
```

Но увидим ошибку, которая говорит о том, что мы сделали локальные изменения и их нужно либо запушить, 
либо отправить в stash, прежде чем мы сможем переключиться.

4) Для этого используем команду:

```
git stash
```
Если мы пропишем git status, то увидим, что наши изменения пропали. На деле они находятся в stash.

Теперь мы сможем переключиться на develop.

```
Основные команды:

1) git stash - отправка изменений в стэш

2)git stash push -m "Временные изменения" — Отправка изменений в стэш с указанием сообщения;

3)git stash list — список сохранённых стэшей;

4)git stash apply — применение последнего стэш;

5)git stash pop — применение и удаление последнего стэша;

6)git stash apply stash@{n} — применение конкретного стэша;

7)git stash drop stash@{n} — если 0, то удаление всех стэшей, другое число — удаление определенного стэша;

8)git stash clear — очистка всех стэшей.

```

## Слияние изменений в веток

```
git merge
```

Команда используется для слияния изменений из одной ветки в другую. 
Эта команда объединяет истории коммитов двух веток, создавая новый коммит (по умолчанию), который включает 
изменения из обеих веток.

Вольем наши изменения из ветки hotfix/change_clg в develop:


```
git switch develop
git merge --no-ff hotfix/change_clg -m "Слияние изменений из hotfix/change_clg в develop"
git push
```

Очень важно писать --no-ff, чтобы сохранить структуру разработки.

Наглядный пример работы:

![](https://habrastorage.org/r/w780/webt/an/p-/56/anp-56ekldeanloppslwuz0f-0i.png)

Теперь на ветке develop в удаленном и локальном репозитории файлы будут обновлены.

```
git merge <from-branch> --no-ff — создаёт новый коммит слияния, даже если возможно быстрое слияние (fast-forward). Полезно для сохранения истории слияния.

git merge <from-branch> --no-commit — объединяет изменения, но не делает коммит автоматически.

git merge <from-branch> --squash — объединяет все коммиты из сливаемой ветки в один коммит в текущей ветке. Полезно для объединения истории коммитов перед слиянием.

git merge --continue — продолжает слияние после разрешения конфликтов

git merge --abort — прерывает начатое слияние при возникновении конфликтов

```


## Загрузка обновлений из удаленного репозитория 

```
git fetch
```

Команда используется для загрузки обновлений из удаленного репозитория в ваш локальный репозиторий.
В отличие от **git pull**, **git featch** не сливает изменения автоматически в вашу текущую ветку. Вместо этого
она загружается все обновления, которые затем можно просмотреть и слить вручную 

Переключимся обратно на каталог **root**

Загрузим обновления из удаленного репозитория командой **git fetch**

В консоли видим 

```
...
  652b6ff..b585d2a  develop               -> origin/develop
* [new branch]      feature/111_add_print -> origin/feature/111_add_print
* [new branch]      hotfix/change_clg     -> origin/hotfix/change_clg
```

Видим краткую информацию, что *develop* обновился и появились две новые ветки

пишим *git log -1 --oneline* и видим, что коммит не соответствует тому, что должно быть в удаленном репозитории

Пишем *git log origin/develop -1 --oneline* и видим последний коммит

Если запустить с флагом --purne, то он уберет локальные ссылки на удаленный ветки, которых уже нет на сервере

---

## Извлечение и слияние изменений 

```
git pull
```

Команда используется для извлечения и слияния изменений из удаленного репозитория в вашу текущую ветку. Эта команда
объединяет две операции *git fetch* и *git merge* Сначала она извлекает изменения из удаленного репозитория 
(аналогично *git fetch*), а затем автоматически сливает их с текущей веткой (аналогично *git merge*)

Выполним команду *git pull* и наша локальная ветка *develop* будет соответствоватьудаленной ветке *origin/develop*

Переключим на ветку *feature/111_add_print* и вытянем изменения, которые мы сделали в *develop*

```
git switch -t origin/feature/111_add_print
git merge develop 
```

Как мы можем увидеть, у нас конфликты:

```
warning: Cannot merge binary files: index.js (HEAD vs. develop)
Auto-merging index.js
CONFLICT (content): Merge conflict in index.js
Automatic merge failed; fix conflicts and then commit the result.
```

Сам же файл:

```
<<<<<<< HEAD
console.log("Вы успешно вошли")
=======
console.log(pass)
function Print(){}
>>>>>>> 2c0279a (Add print function)
```
Вы можете отредактировать файл вручную, удалив строки <<<<<<< HEAD, =======, >>>>>>> 742b087 (Add print function)
 и внести нужные изменения, сохраняя только те строки, которые вы хотите оставить.

Или Вам предложат открыть Merge editor (возможно автоматически откроется), чтобы решить данный конфликт. 
В нем current будет ветка develop, incoming — feature/111_add_print, а result — результат слияния. 
Можно выбрать оба изменения, либо одно, нажав на соответствующие галочки, либо написать что-то свое и нажать 
на Complete Merge. Я добавлю оба изменения:


```
console.log("Вы успешно вошли")
function Print(){}

```
Если мы пропишем git status, то увидим:

```
git status
On branch feature/111_add_print
Your branch is up to date with 'origin/feature/111_add_print'.

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   index.js
```

Как видно мы в состояния merging (you are still merging), нужно завершить merge, просто пишем:

```
git merge --continue
```
---

## Git rebase

```
git rebase
```
Команда используется для интеграции изменений из одной ветки в другую. Она позволяет перенести или 
"переписать" серию коммитов из одной ветки в другую, сохраняя линейную историю. Это полезно для упрощения 
истории проекта и устранения ненужных коммитов слияния.

В Visual Studio Code (VS Code) можно наглядно увидеть графическую визуализацию истории коммитов на ветке develop:

![](https://habrastorage.org/r/w780/webt/xw/o4/mo/xwo4moum5emuxtcm-drwvhuv-yg.png)

Или при помощи этих команд:

```
git log --oneline
    или
git log --oneline --graph --decorate --all

```

Возьмем последние два коммита:

```
git rebase -i HEAD~2
```

```
pick abac516 Add print function
pick 5201b7f Убрал pass в console.log()
pick 8699ff2 Make print

# Rebase 1e6c9c2..aeba1fd onto 1e6c9c2 (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
...

```

### Основные команды

```
p, pick <commit> — оставить коммит как есть

r, reword <commit> — оставить изменения, но изменить сообщение коммита

e, edit <commit> — остановиться на этом коммите для правок (можно добавить/изменить файлы)

s, squash <commit> — объединить с предыдущим коммитом (сохраняя сообщение текущего)

f, fixup [-C | -c] <commit> — объединить с предыдущим, но игнорировать его сообщение

d, drop <commit> — удалить коммит (опасно — приведёт к потере изменений)
```

**Если ты хочешь изменить историю, но не удалить изменения, то никогда не используй drop или fixup
 без понимания последствий.**



