## Finding with Git Grep ##

Поиск файлов содержащих слово или фразу в Git очень просто с помощью команды
linkgit:git-grep[1]. Это возможно проделать это с помощью обычных Unix команды
'grep', но с помощью 'git grep' вы можете искать также в предыдущих версиях проекта без его чекоута в рабочую директорию.

Например, если Я захотел увидеть каждое место где был использован вызов 'xmmap' в моем репозитории git.git, Я могу выполнить это:

	$ git grep xmmap
	config.c:               contents = xmmap(NULL, contents_sz, PROT_READ,
	diff.c:         s->data = xmmap(NULL, s->size, PROT_READ, MAP_PRIVATE, fd, 0);
	git-compat-util.h:extern void *xmmap(void *start, size_t length, int prot, int fla
	read-cache.c:   mmap = xmmap(NULL, mmap_size, PROT_READ | PROT_WRITE, MAP_PRIVATE,
	refs.c: log_mapped = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, logfd, 0);
	sha1_file.c:    map = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, fd, 0);
	sha1_file.c:    idx_map = xmmap(NULL, idx_size, PROT_READ, MAP_PRIVATE, fd, 0);
	sha1_file.c:                    win->base = xmmap(NULL, win->len,
	sha1_file.c:                    map = xmmap(NULL, *size, PROT_READ, MAP_PRIVATE, f
	sha1_file.c:            buf = xmmap(NULL, size, PROT_READ, MAP_PRIVATE, fd, 0);
	wrapper.c:void *xmmap(void *start, size_t length,

Если Я захотел увидеть также номер линии каждого совпадения, я могу добавить параметр '-n':

	$>git grep -n xmmap
	config.c:1016:          contents = xmmap(NULL, contents_sz, PROT_READ,
	diff.c:1833:            s->data = xmmap(NULL, s->size, PROT_READ, MAP_PRIVATE, fd,
	git-compat-util.h:291:extern void *xmmap(void *start, size_t length, int prot, int
	read-cache.c:1178:      mmap = xmmap(NULL, mmap_size, PROT_READ | PROT_WRITE, MAP_
	refs.c:1345:    log_mapped = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, logfd, 0);
	sha1_file.c:377:        map = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, fd, 0);
	sha1_file.c:479:        idx_map = xmmap(NULL, idx_size, PROT_READ, MAP_PRIVATE, fd
	sha1_file.c:780:                        win->base = xmmap(NULL, win->len,
	sha1_file.c:1076:                       map = xmmap(NULL, *size, PROT_READ, MAP_PR
	sha1_file.c:2393:               buf = xmmap(NULL, size, PROT_READ, MAP_PRIVATE, fd
	wrapper.c:89:void *xmmap(void *start, size_t length,

Если мы заинтересованы и имени файла, мы можем передать параметр '--name-only':

	$>git grep --name-only xmmap
	config.c
	diff.c
	git-compat-util.h
	read-cache.c
	refs.c
	sha1_file.c
	wrapper.c

Мы также можем увидеть как много линий совпадает у нас в каждом файле с помощью параметра '-c':

	$>git grep -c xmmap
	config.c:1
	diff.c:1
	git-compat-util.h:1
	read-cache.c:1
	refs.c:1
	sha1_file.c:5
	wrapper.c:1

Теперь, если Я хочу увидеть где это было использовано в определенной версии git, Я могу добавить ссылку таг в конец:

	$ git grep xmmap v1.5.0
	v1.5.0:config.c:                contents = xmmap(NULL, st.st_size, PROT_READ,
	v1.5.0:diff.c:          s->data = xmmap(NULL, s->size, PROT_READ, MAP_PRIVATE, fd,
	v1.5.0:git-compat-util.h:static inline void *xmmap(void *start, size_t length,
	v1.5.0:read-cache.c:                    cache_mmap = xmmap(NULL, cache_mmap_size, 
	v1.5.0:refs.c:  log_mapped = xmmap(NULL, st.st_size, PROT_READ, MAP_PRIVATE, logfd
	v1.5.0:sha1_file.c:     map = xmmap(NULL, st.st_size, PROT_READ, MAP_PRIVATE, fd, 
	v1.5.0:sha1_file.c:     idx_map = xmmap(NULL, idx_size, PROT_READ, MAP_PRIVATE, fd
	v1.5.0:sha1_file.c:                     win->base = xmmap(NULL, win->len,
	v1.5.0:sha1_file.c:     map = xmmap(NULL, st.st_size, PROT_READ, MAP_PRIVATE, fd, 
	v1.5.0:sha1_file.c:             buf = xmmap(NULL, size, PROT_READ, MAP_PRIVATE, fd

Мы можем видеть что есть некоторые отличия между текущими строками и строками в версии 1.5.0, одно из которых это то что xmmap теперь используется в wrapper.c в отличии от версии v1.5.0 где это было не так.

Мы также можем комбинировать условия поиска в grep. Положим мы захотели найти где в нашем репозитории определена SORT_DIRENT:

	$ git grep -e '#define' --and -e SORT_DIRENT
	builtin-fsck.c:#define SORT_DIRENT 0
	builtin-fsck.c:#define SORT_DIRENT 1

Мы также можем поискать каждый файл который удовлетворяет *both* условиям поиска, но отобразив каждую строку которая имеет или условие *either* в этих файлах:

	$ git grep --all-match -e '#define' -e SORT_DIRENT
	builtin-fsck.c:#define REACHABLE 0x0001
	builtin-fsck.c:#define SEEN      0x0002
	builtin-fsck.c:#define ERROR_OBJECT 01
	builtin-fsck.c:#define ERROR_REACHABLE 02
	builtin-fsck.c:#define SORT_DIRENT 0
	builtin-fsck.c:#define DIRENT_SORT_HINT(de) 0
	builtin-fsck.c:#define SORT_DIRENT 1
	builtin-fsck.c:#define DIRENT_SORT_HINT(de) ((de)->d_ino)
	builtin-fsck.c:#define MAX_SHA1_ENTRIES (1024)
	builtin-fsck.c: if (SORT_DIRENT)

Мы также можем поискать строки которые содержут один термин и оба двух других терминов, например мы захотели увидеть где мы определили постоянные которые имеют и тот и другой PATH или MAX в имени:

	$ git grep -e '#define' --and \( -e PATH -e MAX \) 
	abspath.c:#define MAXDEPTH 5
	builtin-blame.c:#define MORE_THAN_ONE_PATH      (1u<<13)
	builtin-blame.c:#define MAXSG 16
	builtin-describe.c:#define MAX_TAGS     (FLAG_BITS - 1)
	builtin-fetch-pack.c:#define MAX_IN_VAIN 256
	builtin-fsck.c:#define MAX_SHA1_ENTRIES (1024)
	...
	

Что здесь имелось ввиду под словом term - терминн или условие или просто слово