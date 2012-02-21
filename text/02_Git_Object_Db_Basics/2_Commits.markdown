### Объекты коммит ###

Объект "коммит" связывает физическое состояние дерева с описпнием того каким образом мы пришли к этому и почему. 

[fig:object-commit]

Вы можете использовать параметр --pretty=raw с linkgit:git-show[1] или linkgit:git-log[1] чтобы исследовать коммит:

    $ git show -s --pretty=raw 2be7fcb476
    commit 2be7fcb4764f2dbcee52635b91fedb1b3dcf7ab4
    tree fb3a8bdd0ceddd019615af4d57a53f43d8cee2bf
    parent 257a84d9d02e90447b149af58b271c19405edb6a
    author Dave Watson <dwatson@mimvista.com> 1187576872 -0400
    committer Junio C Hamano <gitster@pobox.com> 1187591163 -0700

        Fix misspelling of 'suppress' in docs

        Signed-off-by: Junio C Hamano <gitster@pobox.com>

Как вы это видите коммит определяется:

- **дерево**: SHA1 имя объекта дерево (как определено ниже), представляет
  непосредственно предыдущий шаг(и) в определенный момент времени.
- **родитель(и)**: SHA1 имя некоторого числа коммитов которые представляют 
  собой непосредственнцю предыдущую шаг(и) в истории проекта. Пример выше имеет одного родителя; коммиты слияния могут иметь более чем одного родителя. Коммит без родителей называется "root (корневой)" коммит, и представляет собой начальное состояние проекта. Каждый проект должен иметь по крайней мере один корневой коммит. Проект может также иметь множество корней, хотя это не общий случай (или не обязательно хорошая идея).
- an **author**: The name of the person responsible for this change, together
  with its date.
- **коммитер**: имя разработчика которая создала этот коммит, вместе с датой
  этого события. Оно(имя) может отличаться от имени автора; например в случае, если автор написал патч и отправил его по эл.почте другому разработчику который наложил патч и выполнил коммит.
- **комментарий** описывающий этот коммит.

Заметьте что коммит сам по себе не содержит никакой информации о том что изменилось; все изменения вычистляются в сравнении содержимого дерева на которое ссылается этот коммит di all changes are calculated by comparing the contents
of the tree referred to by this commit with the trees associated with
its parents.  In particular, git does not attempt to record file renames
explicitly, though it can identify cases where the existence of the same
file data at changing paths suggests a rename.  (See, for example, the
-M option to linkgit:git-diff[1]).

A commit is usually created by linkgit:git-commit[1], which creates a
commit whose parent is normally the current HEAD, and whose tree is
taken from the content currently stored in the index.

### The Object Model ###

So, now that we've looked at the 3 main object types (blob, tree and commit), 
let's take a quick look at how they all fit together.

If we had a simple project with the following directory structure:

    $>tree
    .
    |-- README
    `-- lib
        |-- inc
        |   `-- tricks.rb
        `-- mylib.rb

    2 directories, 3 files

And we committed this to a Git repository, it would be represented like this:

[fig:objects-example]

You can see that we have created a **tree** object for each directory (including the root)
and a **blob** object for each file.  Then we have a **commit** object to point
to the root, so we can track what our project looked like when it was committed.