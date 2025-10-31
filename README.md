# LaTeX Шаблон для Лабораторной Работы

Готовый шаблон для оформления лабораторных работ в соответствии с требованиями МГТУ "СТАНКИН". Шаблон настроен для работы с XeLaTeX и поддерживает русский язык, кастомные шрифты и автоматическую компиляцию в VS Code.

## Требования

- XeLaTeX (или pdflatex как альтернатива)
- VS Code с расширением LaTeX Workshop
- Установленные шрифты: Liberation Serif, DejaVu Sans Mono (для Linux)

## Файловая архитектура

```
latex-lab-template/
├── main.tex                 # Основной файл документа
├── chapters/                # Папка с содержимым
│   └── chapter1.tex         # Первая глава (используется как шаблон)
├── images/                  # Папка для изображений
│   └── logo.png             # Логотип университета
├── references.bib           # База данных для библиографии (опционально)
├── .vscode/                 # Конфигурация VS Code
│   └── settings.json        # Настройки LaTeX Workshop
├── .gitignore               # Правила игнорирования для Git
└── README.md                # Этот файл
```

### Описание файлов

**main.tex** - основной файл документа, содержит:
- Строка магического комментария для XeLaTeX
- Загрузку всех необходимых пакетов
- Настройки шрифтов и языка
- Оформление титульной страницы с полями для заполнения
- Шаблон оглавления
- Подключение глав через команду `\input{chapters/chapter1.tex}`

**chapters/chapter1.tex** - шаблон содержимого с примером структуры (раздел, подраздел)

**references.bib** - файл для хранения источников в формате BibTeX (используется при включении пакета biblatex)

**.vscode/settings.json** - конфигурация расширения LaTeX Workshop для VS Code с рецептами компиляции и правилами очистки вспомогательных файлов

## Быстрый старт

1. Отредактируйте параметры документа в файле main.tex (строки 61-67):

```latex
\newcommand{\kafedraFirstString}{Название кафедры (строка 1)}
\newcommand{\kafedraSecondString}{Название кафедры (строка 2)}
\newcommand{\discipline}{Название дисциплины}
\newcommand{\numberOfWork}{№1}  % номер лабораторной
\newcommand{\topic}{Тема лабораторной работы}
\newcommand{\teacher}{Фамилия И.О. преподавателя}
\newcommand{\yearr}{2024 г.}
```

2. Добавьте содержимое в файл chapters/chapter1.tex или создайте новые главы

3. При сохранении документ автоматически скомпилируется в PDF

## LaTeX Рецепты компиляции

Рецепт - это последовательность команд, которые выполняются для создания PDF. В шаблоне доступны следующие рецепты:

### xelatex × 2 (по умолчанию)

Компилирует документ два раза подряд с помощью XeLaTeX.

```bash
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -shell-escape main.tex
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -shell-escape main.tex
```

Используйте этот рецепт если в документе есть оглавление, перекрёстные ссылки или гиперссылки. Первый проход собирает информацию, второй - обновляет оглавление и ссылки.

### xelatex × 1

Быстрый проход - одна компиляция с XeLaTeX:

```bash
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -shell-escape main.tex
```

Используйте для быстрой проверки черновика, когда оглавление не критично.

### xelatex → biber → xelatex × 2

Полный цикл компиляции с обработкой библиографии:

```bash
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -shell-escape main.tex
biber main
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -shell-escape main.tex
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -shell-escape main.tex
```

Используйте этот рецепт ТОЛЬКО если вы включили пакет biblatex в main.tex.

### pdflatex × 2

Компиляция с помощью pdflatex (альтернатива XeLaTeX):

```bash
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error main.tex
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error main.tex
```

Используйте если XeLaTeX не работает или недоступен.

### pdflatex × 1

Быстрая компиляция с pdflatex:

```bash
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error main.tex
```

## Как использовать шаблон

### Редактирование содержимого

Основное содержимое документа находится в файлах папки chapters/. Каждый файл должен содержать одну или несколько секций (\section) и подсекций (\subsection).

Чтобы добавить новую главу:

1. Создайте файл chapter2.tex в папке chapters/
2. Напишите содержимое:

```latex
\section{Название второй главы}

\subsection{Подраздел}

Ваш текст здесь...
```

3. Подключите главу в main.tex после строки с \input{chapters/chapter1.tex}:

```latex
\input{chapters/chapter2.tex}
```

### Вставка изображений

Все изображения должны находиться в папке images/. Для вставки используйте команду:

```latex
\includegraphics[width=0.5\linewidth]{image_name.png}
```

Путь относительный, начинается от корневой папки проекта.

### Автоматическая компиляция

При сохранении файла в VS Code:
1. Расширение LaTeX Workshop автоматически запускает выбранный рецепт компиляции
2. PDF обновляется в отдельной вкладке VS Code
3. После компиляции автоматически удаляются вспомогательные файлы (.aux, .log, .toc и т.д.)

### Синхронизация кода и PDF (SyncTeX)

После включения SyncTeX можно:
- Двойной клик в PDF-просмотре перейдёт к соответствующему месту в коде
- Правый клик в коде > "Go to PDF" перейдёт к соответствующему месту в PDF

## Настройка шрифтов

В preamble шаблона (строки 9-19) настроены шрифты для Linux:

```latex
\setmainfont{Liberation Serif}        % основной шрифт
\setmonofont{DejaVu Sans Mono}[Scale=MatchLowercase]  % моноширинный шрифт
```

Если вы на Windows или macOS, раскомментируйте альтернативные строки:

```latex
% Для Windows/macOS
\setmainfont{Times New Roman}
\setmonofont{Courier New}
```

## Подключение дополнительных возможностей

### Библиография (Biber + Biblatex)

Раскомментируйте строки 49-50 в main.tex:

```latex
\usepackage[backend=biber, style=gost-numeric, language=autobib, autolang=other]{biblatex}
\addbibresource{references.bib}
```

Раскомментируйте также раздел вывода библиографии в конце документа (строки 174-188).

Для компиляции используйте рецепт "xelatex → biber → xelatex × 2".

Добавляйте источники в references.bib в формате:

```bibtex
@book{key,
  author = {Фамилия, Имя},
  title = {Название книги},
  year = {2024}
}
```

В документе ссылайтесь на источник: \cite{key}

### Графики с PGFPlots

Раскомментируйте строки 53-54:

```latex
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
```

### Вывод кода с подсветкой синтаксиса

Раскомментируйте строки 57-58:

```latex
\usepackage{listings}
\usepackage{xcolor}
```

## VS Code настройки

Основные параметры расширения LaTeX Workshop в .vscode/settings.json:

- `autoBuild.run: onSave` - компиляция при каждом сохранении файла
- `autoClean.run: onBuilt` - автоматическая очистка вспомогательных файлов после компиляции
- `view.pdf.viewer: tab` - отображение PDF в отдельной вкладке VS Code
- `synctex.afterBuild.enabled: true` - синхронизация между кодом и PDF

Все рецепты определены в параметре `latex.recipes`.
