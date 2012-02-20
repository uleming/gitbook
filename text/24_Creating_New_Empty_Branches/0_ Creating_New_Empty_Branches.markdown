## Создание новых пустых ветвей ##

Иногда, вам возможно понадобится ветки в вашем репозитории которые не расшаривают предка с вашим обычным кодом. Пример такого случая сгенерированная документация или что нибудь в этом роде. Если вам требуется создать новую ветку которая не содержит ваш текущий код как родителя, вы можете это проделать след.образом:

    git symbolic-ref HEAD refs/heads/newbranch 
    rm .git/index 
    git clean -fdx 
    <do work> 
    git add your files 
    git commit -m 'Initial commit'
    
[gitcast:c9-empty-branch]("GitCast #7: Creating Empty Branches")
