## Настройка Git ##

linkgit:git-config[1]

### Изменение вашего редактора ###

Это изменит редактор вызываемый git по умолчанию

	$ git config --global core.editor emacs

### Добавление алиасов ###
	
	$ git config --global alias.last 'cat-file commit HEAD'
	
	$ git last
	tree c85fbd1996b8e7e5eda1288b56042c0cdb91836b
	parent cdc9a0a28173b6ba4aca00eb34f5aabb39980735
	author Scott Chacon <schacon@gmail.com> 1220473867 -0700
	committer Scott Chacon <schacon@gmail.com> 1220473867 -0700

	fixed a weird formatting problem
	
	$ git cat-file commit HEAD
	tree c85fbd1996b8e7e5eda1288b56042c0cdb91836b
	parent cdc9a0a28173b6ba4aca00eb34f5aabb39980735
	author Scott Chacon <schacon@gmail.com> 1220473867 -0700
	committer Scott Chacon <schacon@gmail.com> 1220473867 -0700

	fixed a weird formatting problem

### Добавление цветов ###

Просмотрите все параметры документации color.* linkgit:git-config[1]

	$ git config color.branch auto
	$ git config color.diff auto
	$ git config color.interactive auto
	$ git config color.status auto

Или, вы можете установить их все с помощью параметра color.ui:

	$ git config color.ui true
	
### Коммит шаблон ###

	$ git config commit.template '/etc/git-commit-template'
	
### Форматирование лога ###

	$ git config format.pretty oneline


### Другие настройки ###

Существуют также и другие интересные настройки для операций упаковки, gc-ing(сборщик мусора git), слияния, удаленнок, ветвей, http transport, diffs, страниц, пробелов и многие другие.  Если вы хотите настроить их, просмотрите документацию linkgit:git-config[1].