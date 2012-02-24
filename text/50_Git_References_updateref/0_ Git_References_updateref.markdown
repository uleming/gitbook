## Git Справочник ##

Ветви, удаленно-отслеживаемые ветки, и таги все ссылаются на коммиты. Все ссылки именуются со слэш разделением имени пути начинающегося с "refs"; имена которыми мы пользуемся до сих пор фактически сокращения:

	- The branch "test" is short for "refs/heads/test".
	- The tag "v2.6.18" is short for "refs/tags/v2.6.18".
	- "origin/master" is short for "refs/remotes/origin/master".

Полное имя иногда полезно, если например, существует ветка и таг с одинаковым именем.

(Только что созданные ссылки в действительности хранятся в директории .git/refs, по пути заданному по их имени. Как бы там ни было, для большей эффективности они могут быть также упакованы вместе в единый файл; просмотрите linkgit:git-pack-refs[1]).

Другое полезное сокращение, сслыку на "HEAD" репозитория можно получить просто используя имя этого репозитория. Например имя "origin" обычно сокращение для HEAD ветки в репозитории "origin".

Для полного списка путей которые git проверяет на ссылки, и порядок который он использует чтобы решать что выбрать когда есть несколько ссылок с одним сокращенным именем, просмотрите секцию "SPECIFYING REVISIONS" linkgit:git-rev-parse[1].


### Отобразить коммиты уникальные для данной ветки ###

Предположим вам хочется увидеть все коммиты достижимые из головы ветки с именем "master", но не из любой другой головы в вашем репозитории.

Мы можем получить список HEAD(ветвей) в нашем репозитории с помощью linkgit:git-show-ref[1]:

    $ git show-ref --heads
    bf62196b5e363d73353a9dcf094c59595f3153b7 refs/heads/core-tutorial
    db768d5504c1bb46f63ee9d6e1772bd047e05bf9 refs/heads/maint
    a07157ac624b2524a059a3414e99f6f44bebc1e7 refs/heads/master
    24dbc180ea14dc1aebe09f14c8ecf32010690627 refs/heads/tutorial-2
    1e87486ae06626c2f31eaa63d26fc0fd646c8af2 refs/heads/tutorial-fixes

Мы можем получить просто имена HEAD ветвей, и удалить "master", с помощью стандартных утилит cut и grep:

    $ git show-ref --heads | cut -d' ' -f2 | grep -v '^refs/heads/master'
    refs/heads/core-tutorial
    refs/heads/maint
    refs/heads/tutorial-2
    refs/heads/tutorial-fixes

И затем мы можем запросить просмотреть все коммиты достижимые из ветки master но не из других HEAD ветвей:

    $ gitk master --not $( git show-ref --heads | cut -d' ' -f2 |
    				grep -v '^refs/heads/master' )

Очевидно, что тут возможны бесконечные вариации; например чтобы увидеть все коммиты достижимые из некоторой HEAD ветки но не из какого либо тага в репозитории:

    $ gitk $( git show-ref --heads ) --not  $( git show-ref --tags )

(Просмотрите linkgit:git-rev-parse[1] чтобы получить разяснение синтаксиса commit-selecting такого как `--not`.)

(!!update-ref!!)
