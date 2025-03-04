### Lab02

### Отчёт

---

#### 1. **Подготовка окружения**

```bash
export GITHUB_USERNAME=******
export GITHUB_EMAIL=**************************
export GITHUB_TOKEN=************************************
alias edit=nano
```

- **`GITHUB_USERNAME`** — имя  на GitHub.
- **`GITHUB_EMAIL`** — email для идентификации в Git.
- **`GITHUB_TOKEN`** — персональный токен для аутентификации при работе с GitHub.
- **`alias edit=nano`** — задает текстовый редактор для редактирования файлов.

Затем переходим в рабочую директорию:

```bash
cd ${GITHUB_USERNAME}/workspace
```

---

#### 2. **Настройка Git и GitHub**
Для работы с GitHub через командную строку настраивается утилита `hub`:

```bash
mkdir ~/.config
cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
git config --global hub.protocol https
```

- Создается файл конфигурации `hub` с указанием имени пользователя, токена и протокола HTTPS.
- Устанавливается глобальная настройка протокола для `hub`.

Далее настраиваются глобальные параметры Git:

```bash
git config --global user.name ${GITHUB_USERNAME}
git config --global user.email ${GITHUB_EMAIL}
```

- Эти команды задают имя и email для всех коммитов 
### (коммит в Git - это действие сохранения изменений, внесённых в файлы проекта, в истории репозитория).

---

#### 3. **Создание репозитория для лабораторной работы**
Создаём директорию для проекта и инициализирует Git-репозиторий:

```bash
mkdir -p projects/lab02 && cd projects/lab02
git init

#Переинициализирован существующий репозиторий Git в /Users/vladhodorovskij/SLoMpi/workspace/projects/lab02/.git/
```

Добавляем удаленный репозиторий и создает начальный файл `README.md`:

```bash
git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
git add README.md
git commit -m "added README.md"
#[master 4d3dd1d] added README.md
git push origin master

#Перечисление объектов: 5, готово.
#Подсчет объектов: 100% (5/5), готово.
#При сжатии изменений используется до 8 потоков
#Сжатие объектов: 100% (2/2), готово.
#Запись объектов: 100% (3/3), 2.23 КиБ | 2.23 МиБ/с, готово.
#Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
#To https://github.com/SLoMpi/lab02.git
#   d25dc35..4d3dd1d  master -> master
```

- Если репозиторий уже существует, команда `git remote add` вызовет ошибку, но это не мешает процессу.
- Я ввожу свои учетные данные (имя и токен), чтобы выполнить `push`.

---

#### 4. **Обновление локального репозитория**
После добавления файла `.gitignore` на GitHub (через веб-интерфейс), я синхронизирую изменения:

```bash
git pull origin master
```

- Это обновляет локальный репозиторий, включая новый `.gitignore`.

---

#### 5. **Создание файлов проекта**
Добавляю исходные файлы в проект:

```bash
mkdir sources include examples
cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF

cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF

cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF

cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

- Создаются директории `sources`, `include`, `examples` и файлы с кодом.

Затем редактируется `README.md`:

```bash
nano README.md
```

И изменения отправляются в репозиторий:

```bash
git status

#Текущая ветка: master
#Изменения, которые не в индексе для коммита:
#  (используйте «git add <файл>...», чтобы добавить файл в индекс)
#  (используйте «git restore <файл>...», чтобы отменить изменения в рабочем каталоге)
#	изменено:      README.md
#
#Неотслеживаемые файлы:
#  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
#	.DS_Store
#	examples/
#	include/
#	reports/
#	sources/
#	tasks/
#
#индекс пуст (используйте «git add» и/или «git commit -a»)

git add *

#warning: добавление встроенного git репозитория: tasks/lab02
#hint: You've added another git repository inside your current repository.
#hint: Clones of the outer repository will not contain the contents of
#hint: the embedded repository and will not know how to obtain it.
#hint: If you meant to add a submodule, use:
#hint:
#hint: 	git submodule add <url> tasks/lab02
#hint:
#hint: If you added this path by mistake, you can remove it from the
#hint: index with:
#hint:
#hint: 	git rm --cached tasks/lab02
#hint:
#hint: See "git help submodule" for more information.
#hint: Disable this message with "git config set advice.addEmbeddedRepo false"

git commit -m "added sources"

#[master 1755eea] added sources
# 9 files changed, 241 insertions(+), 2 deletions(-)
# create mode 100644 .DS_Store
# create mode 100644 examples/example1.cpp
# create mode 100644 examples/example2.cpp
# create mode 100644 include/print.hpp
# create mode 100644 reports/.DS_Store
# create mode 100644 reports/lab02/REPORT.md
# create mode 100644 sources/print.cpp
# create mode 160000 tasks/lab02

git push origin master

#Перечисление объектов: 18, готово.
#Подсчет объектов: 100% (18/18), готово.
#При сжатии изменений используется до 8 потоков
#Сжатие объектов: 100% (12/12), готово.
#Запись объектов: 100% (16/16), 4.24 КиБ | 4.24 МиБ/с, готово.
#Total 16 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
#remote: Resolving deltas: 100% (2/2), completed with 1 local object.
#To https://github.com/SLoMpi/lab02.git
#   4d3dd1d..1755eea  master -> master
```

---

#### 6. **Публикация отчета через Gist**
Теперь перехожу к ключевой части — публикации отчета:

1. **Переход в корневую директорию и устанавливаем номер л/р**:
   ```bash
   cd ~/workspace/
   export LAB_NUMBER=02
   ```

2. **Клонирование репозитория с задачами**:
   ```bash
   git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
   ```
   - Это загружает материалы лабораторной работы.

3. **Создание директории для отчета**:
   ```bash
   mkdir -p reports/lab${LAB_NUMBER}
   ```

4. **Копирование шаблона отчета**:
   ```bash
   cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
   ```

5. **Редактирование отчета**:
   ```bash
   cd reports/lab${LAB_NUMBER}
   nano REPORT.md
   ```
   - Вношу изменения в отчет.

6. **Публикация через Gist**:
   ```bash
   gist REPORT.md
   ```
   - Утилита `gist` загружает файл `REPORT.md` на GitHub Gist, возвращая ссылку на опубликованный документ.
