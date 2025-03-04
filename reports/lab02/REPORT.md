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
mkdir -p ~/.config
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
```

Добавляем удаленный репозиторий и создает начальный файл `README.md`:

```bash
git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
touch README.md
git add README.md
git commit -m "added README.md"
git push origin master
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
git add .
git commit -m "added sources"
git push origin master
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