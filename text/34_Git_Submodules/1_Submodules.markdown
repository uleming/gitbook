## Подмодули ##

Большие проекты часто составлены из маленьких, самодостаточных модулей. Например, дерево исходных кодов дистрибутива "Встроенный Linux" включает каждую часть кода в обычного дистрибутива с характерными изменениями; видео проигрыватель возможно требует сборку с только определенной, рабочей версии библиотеки декомпресии; несколько независимых программ возможно пользуются совместно одними скриптами сборки.

С централизованной системой контроля версий это часто достигается включением каждого модуля в единый репозиторий. Разработчики могут извлекать все модули или только модули необходимые им для работы в данный момент. Они могут даже модифицировать файлы в несколькоих модулях в единый коммит в то время как перемещают файлы или обновляют API и переводы..

Git не позволяет выполнять частичное извлечение, и дублирование этого подхода в Git заставляет разработчиков держать локальные копии модулей которые они не трогают. Коммиты таких огромных извлечений будут медленнее чем вы ожидаете так как Git должен будет просканировать каждую директорию на наличие изменений. Если модули имеют огромную локальную историю то клонирование может занять вечность.

Одна положительная сторона, распределенные системы контроля версий могут быть граздо легче интегрированы с внешними источниками. В централизованной модели, один произвольный снапшот внешнего проекта экспортирован из его собственной системы контроля версий и затем импортирован в локальную систему контроля версий в ветку поставщика. Вся история скрыта. С распределенной системой контроля версий вы модете склонировать всю внешнуюю историю и и даже больше просто следуя разработке и переслиянию локальных изменений.

Поддержка в Git подмодулей позволяет репозиторию содержать, как поддиректорию, извлеченный внешний проект. Подмодули поддерживают свою подлинность; поддержка подмодулей хранит расположение репозитория подмодуля и ID коммита, так что другие разработчики которые клонируют существующий проект ("superproject")могут легко клонировать все подмодули в единой просмотре. Частичные извлечения superproject возможны: вам нужно указать Git клонировать или ничто или некоторые или все подмодули.

Команда linkgit:git-submodule[1] доступна начиная с версии Git 1.5.3. Пользователи с Git 1.5.2 могут поискать коммиты подмодулей в репозитории и вручную извлечь их; ранние версии Git не узнают подмодули вообще..

Чтобы увидеть как работает поддержка подмодулей, создайте (для примера) 4 образца репозиториев которые могут быть использованы позже как подмодуль:

    $ mkdir ~/git
    $ cd ~/git
    $ for i in a b c d
    do
        mkdir $i
	    cd $i
	    git init
	    echo "module $i" > $i.txt
	    git add $i.txt
	    git commit -m "Initial commit, submodule $i"
	    cd ..
    done

Теперь создайте superproject и добавте все подмодули:

    $ mkdir super
    $ cd super
    $ git init
    $ for i in a b c d
    do
        git submodule add ~/git/$i $i
    done

Замечание: Не используйте здесь локальные URL если вы планируете опубликовать ваш superproject!

Посмотрим какие файлы создал `git-submodule`:

    $ ls -a
    .  ..  .git  .gitmodules  a  b  c  d

Команда выполняте пару вещей `git-submodule add`:

- Она клонирует подмодули под текущей директорией и по умолчанию извлекает
  ветку master.
- Она добавляет путь клона подмодуля к файлу linkgit:gitmodules[5] и добавляет
  этот файл в директорию заморозки, подготавливая все к коммиту.
- Она добавляет текущий ID коммита подмодуля в директорию заморозки, подготавлива все к коммиту..

Коммит superproject:


    $ git commit -m "Add submodules a, b, c and d."

Теперь склонируем superproject:

    $ cd ..
    $ git clone super cloned
    $ cd cloned

Директории подмодуля здесь, но они пустые:

    $ ls -a a
    .  ..
    $ git submodule status
    -d266b9873ad50488163457f025db7cdd9683d88b a
    -e81d457da15309b4fef4249aba9b50187999670d b
    -c1536a972b9affea0f16e0680ba87332dc059146 c
    -d96249ff5d57de5de093e6baff9e0aafa5276a74 d

Замечание: Имена объектов коммит показанные выше будут отличаться от ваших, но они должны совпадать с именами объектов коммит HEAD ваших репозиториев. Вы можете проверить это выполнив `git ls-remote ../git/a`.

Pulling down the submodules is a two-step process. First run `git submodule
init` to add the submodule repository URLs to `.git/config`:

    $ git submodule init

Now use `git-submodule update` to clone the repositories and check out the
commits specified in the superproject:

    $ git submodule update
    $ cd a
    $ ls -a
    .  ..  .git  a.txt

One major difference between `git-submodule update` and `git-submodule add` is
that `git-submodule update` checks out a specific commit, rather than the tip
of a branch. It's like checking out a tag: the head is detached, so you're not
working on a branch.

    $ git branch
    * (no branch)
    master

If you want to make a change within a submodule and you have a detached head,
then you should create or checkout a branch, make your changes, publish the
change within the submodule, and then update the superproject to reference the
new commit:

    $ git checkout master

or

    $ git checkout -b fix-up

then

    $ echo "adding a line again" >> a.txt
    $ git commit -a -m "Updated the submodule from within the superproject."
    $ git push
    $ cd ..
    $ git diff
    diff --git a/a b/a
    index d266b98..261dfac 160000
    --- a/a
    +++ b/a
    @@ -1 +1 @@
    -Subproject commit d266b9873ad50488163457f025db7cdd9683d88b
    +Subproject commit 261dfac35cb99d380eb966e102c1197139f7fa24
    $ git add a
    $ git commit -m "Updated submodule a."
    $ git push

You have to run `git submodule update` after `git pull` if you want to update
submodules, too.

###Pitfalls with submodules

Always publish the submodule change before publishing the change to the
superproject that references it. If you forget to publish the submodule change,
others won't be able to clone the repository:

    $ cd ~/git/super/a
    $ echo i added another line to this file >> a.txt
    $ git commit -a -m "doing it wrong this time"
    $ cd ..
    $ git add a
    $ git commit -m "Updated submodule a again."
    $ git push
    $ cd ~/git/cloned
    $ git pull
    $ git submodule update
    error: pathspec '261dfac35cb99d380eb966e102c1197139f7fa24' did not match any file(s) known to git.
    Did you forget to 'git add'?
    Unable to checkout '261dfac35cb99d380eb966e102c1197139f7fa24' in submodule path 'a'

If you are staging an updated submodule for commit manually, be careful to not
add a trailing slash when specifying the path. With the slash appended, Git
will assume you are removing the submodule and checking that directory's
contents into the containing repository.

    $ cd ~/git/super/a
    $ echo i added another line to this file >> a.txt
    $ git commit -a -m "doing it wrong this time"
    $ cd ..
    $ git add a/
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       deleted:    a
    #       new file:   a/a.txt
    #
    # Modified submodules:
    #
    # * a aa5c351...0000000 (1):
    #   < Initial commit, submodule a
    #

To fix the index after performing this operation, reset the changes and then
add the submodule without the trailing slash.

    $ git reset HEAD A
    $ git add a
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       modified:   a
    #
    # Modified submodules:
    #
    # * a aa5c351...8d3ba36 (1):
    #   > doing it wrong this time
    #

You also should not rewind branches in a submodule beyond commits that were
ever recorded in any superproject.

It's not safe to run `git submodule update` if you've made and committed
changes within a submodule without checking out a branch first. They will be
silently overwritten:

    $ cat a.txt
    module a
    $ echo line added from private2 >> a.txt
    $ git commit -a -m "line added inside private2"
    $ cd ..
    $ git submodule update
    Submodule path 'a': checked out 'd266b9873ad50488163457f025db7cdd9683d88b'
    $ cd a
    $ cat a.txt
    module a

NOTE: The changes are still visible in the submodule's reflog.

This is not the case if you did not commit your changes.

[gitcast:c11-git-submodules]("GitCast #11: Git Submodules")
