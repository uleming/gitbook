## Просмотр истории - Git Log ##

Команда linkgit:git-log[1] может показать список коммитов. Сама по себе она показывает все коммиты достижимые из родительского коммита; но вы можете также сделать более определенный запрос:

    $ git log v2.5..	    # коммиты с (но не дистижимые) v2.5
    $ git log test..master	# коммит достижимый из ветки master но не из test
    $ git log master..test	# коммит достижимый из ветки test но не из master
    $ git log master...test	# коммит достижимый или из test или master, но не 
      						# из обоих
    $ git log --since="2 weeks ago" # коммиты начиная с 2 недель назад
    $ git log Makefile      # коммиты которые модифицировали Makefile
    $ git log fs/		    # коммиты которые модифицировали любой из файлов в 
    						# поддиректории fs/
    $ git log -S'foo()'	    # коммиты которые добавили или удалили любые 							# файловые данные совпадающие со строкой 'foo()' 
    $ git log --no-merges	# не показывать коммиты слияния

Конечно вы можете комбинировать их; следующая команда найдет коммиты начиная с v2.5 которые затрагивают файл Makefile или любой файл в директории fs/:

    $ git log v2.5.. Makefile fs/

Git log покажет список из всех коммитов, начиная с наиболее свежего(по дате) коммита, который удовлетворяет условиям заданным в аргументах команды.

	commit f491239170cb1463c7c3cd970862d6de636ba787
	Author: Matt McCutchen <matt@mattmccutchen.net>
	Date:   Thu Aug 14 13:37:41 2008 -0400

	    git format-patch documentation: clarify what --cover-letter does
    
	commit 7950659dc9ef7f2b50b18010622299c508bfdfc3
	Author: Eric Raible <raible@gmail.com>
	Date:   Thu Aug 14 10:12:54 2008 -0700

	    bash completion: 'git apply' should use 'fix' not 'strip'
	    Bring completion up to date with the man page.
   
Вы также можете попросить git log показать патчи:

    $ git log -p

	commit da9973c6f9600d90e64aac647f3ed22dfd692f70
	Author: Robert Schiele <rschiele@gmail.com>
	Date:   Mon Aug 18 16:17:04 2008 +0200

	    adapt git-cvsserver manpage to dash-free syntax

	diff --git a/Documentation/git-cvsserver.txt b/Documentation/git-cvsserver.txt
	index c2d3c90..785779e 100644
	--- a/Documentation/git-cvsserver.txt
	+++ b/Documentation/git-cvsserver.txt
	@@ -11,7 +11,7 @@ SYNOPSIS
	 SSH:

	 [verse]
	-export CVS_SERVER=git-cvsserver
	+export CVS_SERVER="git cvsserver"
	 'cvs' -d :ext:user@server/path/repo.git co <HEAD_name>

	 pserver (/etc/inetd.conf):

### Статистика в логах ###

Если вы передадите параметр <code>--stat</code> в 'git log', он покажет вам которые файлы изменились в этом коммите и как много строк кода было добавлено и удалено из каждого из них.

	$ git log --stat
	
	commit dba9194a49452b5f093b96872e19c91b50e526aa
	Author: Junio C Hamano <gitster@pobox.com>
	Date:   Sun Aug 17 15:44:11 2008 -0700

	    Start 1.6.0.X maintenance series
    
	 Documentation/RelNotes-1.6.0.1.txt |   15 +++++++++++++++
	 RelNotes                           |    2 +-
	 2 files changed, 16 insertions(+), 1 deletions(-)


### Форматирование вывода log ###

Вы можете форматировать вывод log так как это удобно. Параметр '--pretty' может принимать множество предопределенных значений, таких как 'oneline' (в одну линию):

	$ git log --pretty=oneline
	a6b444f570558a5f31ab508dc2a24dc34773825f dammit, this is the second time this has reverted
	49d77f72783e4e9f12d1bbcacc45e7a15c800240 modified index to create refs/heads if it is not 
	9764edd90cf9a423c9698a2f1e814f16f0111238 Add diff-lcs dependency
	e1ba1e3ca83d53a2f16b39c453fad33380f8d1cc Add dependency for Open4
	0f87b4d9020fff756c18323106b3fd4e2f422135 merged recent changes: * accepts relative alt pat
	f0ce7d5979dfb0f415799d086e14a8d2f9653300 updated the Manifest file

или вы можете получить 'short' (кратко) форматирование:

	$ git log --pretty=short
	commit a6b444f570558a5f31ab508dc2a24dc34773825f
	Author: Scott Chacon <schacon@gmail.com>

	    dammit, this is the second time this has reverted

	commit 49d77f72783e4e9f12d1bbcacc45e7a15c800240
	Author: Scott Chacon <schacon@gmail.com>

	    modified index to create refs/heads if it is not there

	commit 9764edd90cf9a423c9698a2f1e814f16f0111238
	Author: Hans Engel <engel@engel.uk.to>

	    Add diff-lcs dependency

Далее список возможных вариантов 'medium', 'full', 'fuller', 'email' или 'raw'. Тут лучше поэкспериментировать, чтобы выяснить какой наиболее вам подходит. Если ни один из них не удовалетворяет вашим потребностям вы можете создать свой собственный формат задав параметр след.образом '--pretty=format' (просмотрите документацию linkgit:git-log[1] чтобы узнать все форматирующие параметры).

	$ git log --pretty=format:'%h was %an, %ar, message: %s'
	a6b444f was Scott Chacon, 5 days ago, message: dammit, this is the second time this has re
	49d77f7 was Scott Chacon, 8 days ago, message: modified index to create refs/heads if it i
	9764edd was Hans Engel, 11 days ago, message: Add diff-lcs dependency
	e1ba1e3 was Hans Engel, 11 days ago, message: Add dependency for Open4
	0f87b4d was Scott Chacon, 12 days ago, message: merged recent changes:
	
Другая интересная вещь которую вы можете сделать - это визуализировать граф коммитов ипользуя параметр '--graph', след.образом:

	$ git log --pretty=format:'%h : %s' --graph
	* 2d3acf9 : ignore errors from SIGCHLD on trap
	*   5e3ee11 : Merge branch 'master' of git://github.com/dustin/grit
	|\  
	| * 420eac9 : Added a method for getting the current branch.
	* | 30e367c : timeout code and tests
	* | 5a09431 : add timeout protection to grit
	* | e1193f8 : support for heads with slashes in them
	|/  
	* d6016bc : require time for xmlschema

Это даст вас очень легкое для восприятия ASCII представление истории коммитов.


### Упорядочивание Log ###

Вы также можете сортировать вывод в различном ввиде. Заметьте что git log начинает с самого свежего коммита,затем спускается по родительской линии; так как история git может содержать множество независимых ветвей разработки, определенный порядок коммитов может быть произвольным.

Если вам хочется изменить порядок вывода особым образом, вы можете добавить параметр урорядочивающий вывод команды git log.

По умолчанию, коммиты показываются в обратном хронологическом порядке.

Как бы там ни было, вы можете определить '--topo-order', который выведет коммиты в хронологическом порядке (т.е. коммиты потомки будут выведены перед их родителями).
Если мы просмотрим git log для репозитария Grit в --topo-order, то увидим что все линии разработки сгруппированы вместе.

	$ git log --pretty=format:'%h : %s' --topo-order --graph
	*   4a904d7 : Merge branch 'idx2'
	|\  
	| *   dfeffce : merged in bryces changes and fixed some testing issues
	| |\  
	| | * 23f4ecf : Clarify how to get a full count out of Repo#commits
	| | *   9d6d250 : Appropriate time-zone test fix from halorgium
	| | |\  
	| | | * cec36f7 : Fix the to_hash test to run in US/Pacific time
	| | * | decfe7b : fixed manifest and grit.rb to make correct gemspec
	| | * | cd27d57 : added lib/grit/commit_stats.rb to the big list o' files
	| | * | 823a9d9 : cleared out errors by adding in Grit::Git#run method
	| | * |   4eb3bf0 : resolved merge conflicts, hopefully amicably
	| | |\ \  
	| | | * | d065e76 : empty commit to push project to runcoderun
	| | | * | 3fa3284 : whitespace
	| | | * | d01cffd : whitespace
	| | | * | 7c74272 : oops, update version here too
	| | | * | 13f8cc3 : push 0.8.3
	| | | * | 06bae5a : capture stderr and log it if debug is true when running commands
	| | | * | 0b5bedf : update history
	| | | * | d40e1f0 : some docs
	| | | * | ef8a23c : update gemspec to include the newly added files to manifest
	| | | * | 15dd347 : add missing files to manifest; add grit test
	| | | * | 3dabb6a : allow sending debug messages to a user defined logger if provided; tes
	| | | * | eac1c37 : pull out the date in this assertion and compare as xmlschemaw, to avoi
	| | | * | 0a7d387 : Removed debug print.
	| | | * | 4d6b69c : Fixed to close opened file description.

Вы можете также использовать '--date-order', который изначально упорядочивает коммиты по дате коммита. Этот параметр похож на --topo-order в том смысле что он располагает родителей позади потомков, но тем не менее вывод упорядочен по времени коммита. Вы можете видеть что линии разработки не сгруппированы здесь вместе, так что они скачут вокруг в процессе паралельной разработки:

	$ git log --pretty=format:'%h : %s' --date-order --graph
	*   4a904d7 : Merge branch 'idx2'
	|\  
	* | 81a3e0d : updated packfile code to recognize index v2
	| *   dfeffce : merged in bryces changes and fixed some testing issues
	| |\  
	| * | c615d80 : fixed a log issue
	|/ /  
	| * 23f4ecf : Clarify how to get a full count out of Repo#commits
	| *   9d6d250 : Appropriate time-zone test fix from halorgium
	| |\  
	| * | decfe7b : fixed manifest and grit.rb to make correct gemspec
	| * | cd27d57 : added lib/grit/commit_stats.rb to the big list o' file
	| * | 823a9d9 : cleared out errors by adding in Grit::Git#run method
	| * |   4eb3bf0 : resolved merge conflicts, hopefully amicably
	| |\ \  
	| * | | ba23640 : Fix CommitDb errors in test (was this the right fix?
	| * | | 4d8873e : test_commit no longer fails if you're not in PDT
	| * | | b3285ad : Use the appropriate method to find a first occurrenc
	| * | | 44dda6c : more cleanly accept separate options for initializin
	| * | | 839ba9f : needed to be able to ask Repo.new to work with a bar
	| | * | d065e76 : empty commit to push project to runcoderun
	* | | | 791ec6b : updated grit gemspec
	* | | | 756a947 : including code from github updates
	| | * | 3fa3284 : whitespace
	| | * | d01cffd : whitespace
	| * | | a0e4a3d : updated grit gemspec
	| * | | 7569d0d : including code from github updates


В заключении, вы можете изменить порядок вывода на обратный используя параметр '--reverse'.


[gitcast:c4-git-log]("GitCast #4: Git Log")
