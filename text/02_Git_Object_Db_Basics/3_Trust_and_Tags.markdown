
### Объект таг ###

[fig:object-tag]

Объект таг содержит имя объекта (называетмого просто 'объект'), тип объекта, имя тага, или разработчика ("таггер") который создал таг, и сообщение, которое может содержить подпись. Это можно увидеть выполнив linkgit:git-cat-file[1]:

    $ git cat-file tag v1.5.0
    object 437b1b20df4b356c9342dac8d38849f24ef44f27
    type commit
    tag v1.5.0
    tagger Junio C Haеmano <junkio@cox.net> 1171411200 +0000

    GIT 1.5.0
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v1.4.6 (GNU/Linux)

    iD8DBQBF0lGqwMbZpPMRm5oRAuRiAJ9ohBLd7s2kqjkKlq1qqC57SbnmzQCdG4ui
    nLE/L9aUXdWeTFPron96DLA=
    =2E+0
    -----END PGP SIGNATURE-----

Просмотрите документацию команды linkgit:git-tag[1] чтобы изучить как создавать и проверять объекты таг. (Заметьте что linkgit:git-tag[1] может также использоваться чтобы создавать "легковесные таги", которые не являеются объектами таг вообще, это просто ссылки чьи имена начинаются с "refs/tags/").
