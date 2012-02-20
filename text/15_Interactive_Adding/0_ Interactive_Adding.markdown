## Интерактивное Добавление ##

Интерактивное Добавление это удобный способ работы с и визуализацией директории заморозки Git. Что бы запустить его просто напечатайте 'git add -i'. Git автоматически покажет вам ваши измененные файлы и их статус.

	$>git add -i
	           staged     unstaged path
	  1:    unchanged        +4/-0 assets/stylesheets/style.css
	  2:    unchanged      +23/-11 layout/book_index_template.html
	  3:    unchanged        +7/-7 layout/chapter_template.html
	  4:    unchanged        +3/-3 script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown

	*** Commands ***
	  1: status	  2: update	  3: revert	  4: add untracked
	  5: patch	  6: diff	  7: quit	  8: help
	What now> 

В этом случае, мы можем видеть, что у нас есть 5 измененных файлов котороые еще не добавлены в нашу директорию заморозки (т.е. их статус unstaged), и даже как много строк было добавлено или удалено в каждый файл. Затем покажется интерактивное меню того что мы можем сделать в этом режиме.

Если мы хотим добавить эти файлы в директорию заморозки, мы можем напечатать '2' или 'u' что бы перейти в режим обновления. Затем Я могу определить какие файлы заморозить (добавить в директорию заморозки) напечатав цифру по числу файлов (в нашем случае, 1-4)

	What now> 2
	           staged     unstaged path
	  1:    unchanged        +4/-0 assets/stylesheets/style.css
	  2:    unchanged      +23/-11 layout/book_index_template.html
	  3:    unchanged        +7/-7 layout/chapter_template.html
	  4:    unchanged        +3/-3 script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	Update>> 1-4
	           staged     unstaged path
	* 1:    unchanged        +4/-0 assets/stylesheets/style.css
	* 2:    unchanged      +23/-11 layout/book_index_template.html
	* 3:    unchanged        +7/-7 layout/chapter_template.html
	* 4:    unchanged        +3/-3 script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	Update>> 

Если Я нажму клавишу enter, Я попаду назад в главное меню где Я могу увидеть что статус файла изменился:

	What now> status
	           staged     unstaged path
	  1:        +4/-0      nothing assets/stylesheets/style.css
	  2:      +23/-11      nothing layout/book_index_template.html
	  3:        +7/-7      nothing layout/chapter_template.html
	  4:        +3/-3      nothing script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown

Теперь мы можем видеть первые четыре файла заморожены а последний все еще нет. 
Это по сути краткий способ увидеть ту же информацию которую можно получить выполнив 'git status':

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	modified:   assets/stylesheets/style.css
	#	modified:   layout/book_index_template.html
	#	modified:   layout/chapter_template.html
	#	modified:   script/pdf.rb
	#
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	#

Есть несколько полезных вещей которые мы можем сделать, включая unstaging(разморозку) файлов (3: reverse - откат), добавление неослеживаемых файлов (4:add untracked - добавление неослеживаемых), и просмотр изменений (6: diff - изменения). Эти значения прямо соотвутствуют тому что они делают. Как бы там ни было есть одна замечательная команда, которая производит заморозку патчей (5: patch).

Если вы нажмете '5' или 'p' в меню, git покажет вам ваши diff(изменения) патч за патчем и спросит если вы желаете заморозить каждый. Этим способом вы можете в дейстивительности заморзить для коммита часть отредактированного файла. Если вы отредактировали файл и хотите выполнить коммит только той части что изменили пропустив неизмененную часть, или выполнить коммит документации или отделить значительные изменения от просто добавлненных или удаленных пробелов, вы можете выполнять 'git add -i' чтобы легко проделать это.

Здесь Я заморозил некоторые изменения в файле book_index_template.html, но не все из них:

	         staged     unstaged path
	1:        +4/-0      nothing assets/stylesheets/style.css
	2:       +20/-7        +3/-4 layout/book_index_template.html
	3:        +7/-7      nothing layout/chapter_template.html
	4:        +3/-3      nothing script/pdf.rb
	5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	6:    unchanged       +85/-0 text/15_Interactive_Adding/0_ Interactive_Adding.markdown

Когда вы законичили с фиксацией и заморозкой ваших изменений используя 'git add -i', вы просто завершите работу нажав (7: quit - выход) и затем выполните 'git commit' чтобы закоммитить замороженные изменения. Запомните не **not** выполнять 'git commit -a', так это отменит все сделанные вами аккуратные изменения которые вы только что проделали и просто закоммитит все сразу.

[gitcast:c3_add_interactive]("GitCast #3: Interactive Adding")
