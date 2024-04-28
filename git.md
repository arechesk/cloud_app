# Система контроля версий Git

## Установка Git
```bash
sudo apt install git
```

### Проверка установки
```bash
git --version
```

### Документация
```bash
# Общая:
git --help

# Подробная:
git help git

# По определённой команде:
git add --help
```

## Настройка Git
### Настройка имени и email пользователя
```bash
git config --global user.name "John Doe"
git config --global user.email john_doe@at-consulting.ru
```

### Настройка текстового редактора
```bash
git config --global core.editor nano
```

### Просмотр списка глобальных настроек
```bash
git config --global --list
```

## Основные команды

### `git init` - инициализация репозитория
```bash
mkdir ~/git_repo
cd ~/git_repo

git init

ls -tlah
```

### `git status` - просмотр состояния рабочего дерева
```bash
# Создание файла с содержимым
echo 'My Project' > README.md

git status
```

### `git add` - добавление изменённых файлов в индекс (staging area)
```bash
# Добавление указанного файла (или нескольких - через пробел)
git add README.md

# Добавление всех файлов
git add -A
# или
git add .

git status
```

### `git commit` - фиксация изменений
```bash
git commit
# или
git commit -m "add README.md"

git status
```



### `git diff` - просмотр изменений
```bash
# Изменение файла
echo 'test git project' >> README.md
git status

## Просмотр изменений между рабочим деревом и индексом 
git diff

# Добавление файлов в индекс
git add README.md
git status
git diff

# Просмотр проиндексированных изменений
git diff --staged

# Фиксация изменений
git commit -m 'update README.md'
git status
```

### `git commit --amend` - изменение последнего коммита
```bash
# Изменение файла
echo 'some description' >> README.md
git status
git diff
git add README.md

git commit --amend
```

### `git log` - просмотр истории коммитов
```bash
# Просмотр всех коммитов
git log

# Просмотр указанного числа коммитов
git log -n 1

# Просмотр коммитов в определённом формате
git log --pretty=format:"%h %s"
git log --oneline --decorate --graph --all
```

### `git show` - просмотр коммита
```bash
# последнего
git show

# с указанным хэшом
git show 081942f
```

### `git rm` - удаление файла
```bash
git rm tmp.rb
ls -tlah
git status
git commit -m 'remove tmp.rb'
```


## Работа с ветками

### `git branch <branch_name>` - создание ветки
```bash
git branch new_branch
```

### `git checkout` - переключение на ветку
```bash
git checkout new_branch
```

### `git checkout -b` - создание и переключение на ветку
```bash
git checkout -b new_branch_2
```

### `git branch` - просмотр списка веток
```bash
git branch
```

### `git tag` - создание тега
```bash
git tag 1.0
echo "issue solved" > ISSUE2
git add ISSUE2
git commit -m "issue 2 solved"
git checkout 1.0
cat ISSUE2
```



### `git rebase` - слияние веток (повторное создания коммитов одной ветки поверх другой)
```bash
# Создание ветки experiment
git checkout <second_commit>
git checkout -b experiment

# Добавление коммита в ветку experiment
nano experiment
git add experiment
git commit -m 'add experiment'

git rebase master

git checkout master
git merge experiment
```

## Работа с удалённым репозиторием

### `git clone` - клонирование удалённого репозитория
```bash
git clone git@git-intern.digitalleague.ru:internship/projects/hello_world.git
```

### `git remote add` - добавление удалённого репозитория
```bash
git remote add repo https://host/group/project
```
### `git remote –v` - просмотр списка удалённых репозиториев
```bash
git remote –v
```

### `git fetch` - получение изменений
```bash
git fetch origin branch_name
```

### `git pull` - получение и приненение изменений
```bash
git pull origin branch_name
```

### `git push` - отправка изменений
```bash
git push origin local_branch:remote_branch
git push branch_name
```


Проверить можно командой:
```bash
docker run --rm -v `pwd`:/app ruby:3.0-alpine ruby /app/test.rb
```


