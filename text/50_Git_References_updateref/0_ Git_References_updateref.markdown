## Git Справочник ##

Ветви, удаленно-отслеживаемые ветки, и таги все ссылаются на коммиты. Все ссылки именуются с слэш разделенным имени пути начинающимся с "refs"; имена которыми мы пользуемся до сих пор в действительно короткие
Branches, remote-tracking branches, and tags are all references to
commits.  All references are named with a slash-separated path name
starting with "refs"; the names we've been using so far are actually
shorthand:

	- The branch "test" is short for "refs/heads/test".
	- The tag "v2.6.18" is short for "refs/tags/v2.6.18".
	- "origin/master" is short for "refs/remotes/origin/master".

Полное имя иногда полезно, если например, существует ветка и таг с одинаковым именем.

(Только созданные ссылки в действительности хнанятся в директории .git/refs, по пути данным их по их имени. Как бы там ни было, по причине эффективности они могут также быть упакованы вместе в единый файл; просмотрите linkgit:git-pack-refs[1]).

Другой полезный сокращение, получить ссылку на "HEAD" репозитория можно просто используя имя этого репозитория. Например имя "origin" обычно сокращение для HEAD ветки в репозитории "origin".

Для полного списка путей которые git проверяет на ссылки, и порядок который они использует чтобы решать что выбрать когда есть несколько ссылок с одним сокращенным именем, просмотрите секцию "SPECIFYING REVISIONS" linkgit:git-rev-parse[1].


### Showing commits unique to a given branch ###

Предположим вам хочется увидеть все коммиты достижимые их ветки HEAD которой именуется "master" но не из любой другой головы в вашем репозитории.

Мы можем получить список голов в нашем репозитории с помощью linkgit:git-show-ref[1]:

    $ git show-ref --heads
    bf62196b5e363d73353a9dcf094c59595f3153b7 refs/heads/core-tutorial
    db768d5504c1bb46f63ee9d6e1772bd047e05bf9 refs/heads/maint
    a07157ac624b2524a059a3414e99f6f44bebc1e7 refs/heads/master
    24dbc180ea14dc1aebe09f14c8ecf32010690627 refs/heads/tutorial-2
    1e87486ae06626c2f31eaa63d26fc0fd646c8af2 refs/heads/tutorial-fixes

Мы можем получить просто именаголов ветвей, и удалить "master", с помощью стандартных утилит cut и grep:

    $ git show-ref --heads | cut -d' ' -f2 | grep -v '^refs/heads/master'
    refs/heads/core-tutorial
    refs/heads/maint
    refs/heads/tutorial-2
    refs/heads/tutorial-fixes

И затем мы можем запросить просмотреть все коммиты достижимые из ветки master но не из других голов:

    $ gitk master --not $( git show-ref --heads | cut -d' ' -f2 |
    				grep -v '^refs/heads/master' )

Очевидно, бесконечные вариации возможны; например чтобы увидеть все коммиты достижимые из некоторой головы но не из какого либо тага в репозитории:

    $ gitk $( git show-ref --heads ) --not  $( git show-ref --tags )

(Просмотрите linkgit:git-rev-parse[1] чтобы получить объяснение синтаксиса commit-selecting такого как `--not`.)

(!!update-ref!!)
