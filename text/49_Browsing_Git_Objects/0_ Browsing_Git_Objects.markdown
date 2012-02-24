## Просмотр объектов Git ##

Мы можем запросить git о определенном объекте с помощью команды cat-file. Заметьте что вы можете сократить sha до несколькох символов чтобы не печатать все 40 шестнадцатиричных цифр:

    $ git-cat-file -t 54196cc2
    commit
    $ git-cat-file commit 54196cc2
    tree 92b8b694ffb1675e5975148e1121810081dbdffe
    author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
    committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500

    initial commit

Дерево может ссылаться на один или более объектов, каждому из которых соотвествует файл. Вдобавак, дерево может также ссылаться на другие объекты дерево, таким образом и образуется иерархия директорий. Вы можете исследовать содержимое любого дерева используя команду ls-tree (помните что вполне достаточно набрать начальное значение SHA1):

    $ git ls-tree 92b8b694
    100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad    file.txt

Таким образом мы увидим что это дерево имеет один файл в нем. SHA1 значение это ссылка на файл с данными:

    $ git cat-file -t 3b18e512
    blob

Блоб это просто файл с данными, который мы можем также просмотреть с помощью:

    $ git cat-file blob 3b18e512
    hello world

Заметьте что это старый файл с данными; таким образом объект который git наименовал в ответ на начальное дерево было дерево со снапшотом состояния директории которое было записано первым коммитом.

Все эти объекты хранятся под их SHA1 именами в директории git:

    $ find .git/objects/
    .git/objects/
    .git/objects/pack
    .git/objects/info
    .git/objects/3b
    .git/objects/3b/18e512dba79e4c8300dd08aeb37f8e728b8dad
    .git/objects/92
    .git/objects/92/b8b694ffb1675e5975148e1121810081dbdffe
    .git/objects/54
    .git/objects/54/196cc2703dc165cbd373a65a4dcf22d50ae7f7
    .git/objects/a0
    .git/objects/a0/423896973644771497bdc03eb99d5281615b51
    .git/objects/d0
    .git/objects/d0/492b368b66bdabf2ac1fd8c92b39d3db916e59
    .git/objects/c4
    .git/objects/c4/d59f390b9cfd4318117afde11d601c1085f241

и содержимое этих файлов это просто сжатые данные плюс заголовок идентифицирующий их длину и их тип. Тип это блоб, дерево или коммит или таг.

Простейший коммит для поиска это коммит HEAD, который можно найти из .git/HEAD:

    $ cat .git/HEAD
    ref: refs/heads/master

Вы можете видеть, что это говорит нам в какой ветке мы находимся в данный момент, и это нам известно по имени файла в директории .git, который сам содержит SHA1 имя ссылающееся на объект коммит, который мы можем просмотреть с помощью cat-file:

    $ cat .git/refs/heads/master
    c4d59f390b9cfd4318117afde11d601c1085f241
    $ git cat-file -t c4d59f39
    commit
    $ git cat-file commit c4d59f39
    tree d0492b368b66bdabf2ac1fd8c92b39d3db916e59
    parent 54196cc2703dc165cbd373a65a4dcf22d50ae7f7
    author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143418702 -0500
    committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143418702 -0500

    add emphasis

Объект дерево здесь ссылается на новое состояние дерева:

    $ git ls-tree d0492b36
    100644 blob a0423896973644771497bdc03eb99d5281615b51    file.txt
    $ git cat-file blob a0423896
    hello world!

и объект родитель ссылается на предыдущий коммит:

    $ git-cat-file commit 54196cc2
    tree 92b8b694ffb1675e5975148e1121810081dbdffe
    author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
    committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
