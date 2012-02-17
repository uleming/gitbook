### Установка из исходников ###

Коротко на Unix системах, вы можете скачать исходный код Git со страницы закачки [Git Download Page](http://git-scm.com/download), и затем выполнить следующие команды:

    $ make prefix=/usr all ;# от своего имени
    $ make prefix=/usr install ;# как суперпользователь (root)

Вам потребуются следующие библиотеки:
[expat](http://expat.sourceforge.net/)
[curl](http://curl.linux-mirror.org)
[zlib](http://www.zlib.net)
[openssl](http://www.openssl.org)
хотя за возможным исключением *expat*, они уже будут в системе.