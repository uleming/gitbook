## Интерактивное Rebasing ##

Вы также можете выполнять rebase интерактивно. Это часто используется чтобы переписать ваши объекты коммит перед как выполнить их push. Это простой способ разделить, слить или переупорядочить коммиты перед тем как расшарить их с другими. Вы также можете использовать их чтобы чистить коммиты которые вы вытянули у кого либо и затем применить их на локальном репозитории..

Если у вас есть некоторое число коммитов которые вы хотели бы модифицировать во время выполнения rebase, вы можете вызвать интерактивный диалог передав параметр '-i' или '--interactive' в команду 'git rebase'.

	$ git rebase -i origin/master
	
Это вызовет интерактивный режим выполнения операции rebase на все коммиты которые вы сделали с вашего последнего момента выполнения push (или слияния из репозитория origin).

Чтобы увидеть эти коммиты заранее, вы можете выполнить команду git log след. образом:
	
	$ git log github/master..
	
Как только вы выполнили команду 'rebase -i', тут же откроется ваш редактор по умолчанию с текстом похожим на что то в виде:

	pick fc62e55 added file_size
	pick 9824bf4 fixed little thing
	pick 21d80a5 added number to log
	pick 76b9da6 added the apply command
	pick c264051 Revert "added file_size" - not implemented correctly

	# Rebase f408319..b04dc3d onto f408319
	#
	# Commands:
	#  p, pick = use commit
	#  e, edit = use commit, but stop for amending
	#  s, squash = use commit, but meld into previous commit
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	# However, if you remove everything, the rebase will be aborted.
	#

Это означает что было сделано пять коммитов с последнего выполнения вами push и это даст вам одну линию на каждый коммит следующего формата:

	(action) (partial-sha) (short commit message)
	
Теперь, вы можете изменить действие (по умолчанию 'pick'-выбрать) на 'edit'-редактировать или 'squash'- сдавить или просто оставте это как 'pick'. Вы можете также переупорядочить коммиты просто передвигая линии так как вам этого хочется. Затем, когда вы выйдете из редактора, git попытается уложить коммиты тем образом каким вы это определили и выполнить действия определенные вами. 

Если определен 'pick', то он просто попробует применить патч и сохранить коммит с тем же сообщением-описанием как и было до этого.

Если определено 'squash', то он будет комбинировать это коммит с предыдущим чтобы создать новый коммит. Вы опять попадете в редактор чтобы объединить оба сообщение-описания обоих коммитов сдавив их вместе. Таким образом если вы выйдете из редактора с этим:

	pick   fc62e55 added file_size
	squash 9824bf4 fixed little thing
	squash 21d80a5 added number to log
	squash 76b9da6 added the apply command
	squash c264051 Revert "added file_size" - not implemented correctly

То вам нужно создать одно сообщение-описание коммита из этого:

	# This is a combination of 5 commits.
	# The first commit's message is:
	added file_size

	# This is the 2nd commit message:

	fixed little thing

	# This is the 3rd commit message:

	added number to log

	# This is the 4th commit message:

	added the apply command

	# This is the 5th commit message:

	Revert "added file_size" - not implemented correctly

	This reverts commit fc62e5543b195f18391886b9f663d5a7eca38e84.

Как только вы отредактировали это в одно сообщение-описание и вышли из редактора, коммит будет сохранен с вашим новым сообщением-описанием.

Если определен 'edit', то будет делать тоже самое, но приостановится перед тем как двигаться к следующему и выбросит вас в коммандую строку так вы можете изменить коммит, или изменить содержимое коммита некоторым образом.

Если вы хотите разделить коммит, например, вы определите 'edit' для этого коммита:

	pick   fc62e55 added file_size
	pick   9824bf4 fixed little thing
	edit   21d80a5 added number to log
	pick   76b9da6 added the apply command
	pick   c264051 Revert "added file_size" - not implemented correctly

И затем когда вы попадете в командную строку, вы вернетесь к этому коммиту и создадите два (или более) новых. Давайте положим 21d80a5 изменил два файла , file1 и file2, и вы хотите разделить их в разные коммиты. Вы можете сделать это после того как выполнение rebase выкинуло вас в командную строку:

	$ git reset HEAD^
	$ git add file1
	$ git commit 'first part of split commit'
	$ git add file2
	$ git commit 'second part of split commit'
	$ git rebase --continue
	
И теперь вместо 5 коммитов, у вас их 6.

Последняя вещь которая интерактивное выполнение rebase может делать для вас это сбрасывать коммиты.  Если вместо того чтобы выбрать 'pick', 'squash' или 'edit' для линии коммита, вы просто удалите линию, это удалить коммит из истории.