
# О проекте: Кастомная тема и деплой с использованием GitHub Actions

## Описание проекта

Этот проект направлен на создание кастомной темы для сайта на основе MkDocs, а также на настройку пайплайна для автоматической сборки и деплоя сайта с помощью GitHub Actions.

## Структура проекта

- **.github/workflows/deploy.yml** - Конфигурация GitHub Actions для автоматической сборки и деплоя сайта на GitHub Pages.
```yaml
name: Deploy MKDocs

on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Setup Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install mkdocs mkdocs-material

      - name: Install PostCSS and HTML Validator
        run: |
          npm install -g postcss postcss-cli autoprefixer cssnano html-validator-cli

      - name: Build the site
        run: |
          mkdocs build

      - name: Validate and Minify CSS
        run: |
          postcss site/assets/css/styles.css --use autoprefixer cssnano -o site/assets/css/styles.min.css

      - name: Validate HTML
        run: |
          html-validator --file=site/index.html --verbose

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

- **mkdocs.yml** - Основной конфигурационный файл для MkDocs, настроенный на использование кастомной темы.
```yaml
site_name: My Docs
nav:
  - Домашняя: index.md
  - Первая лабораторная: lab1.md
  - Вторая лабораторная: lab2.md
  - Контакты: contact.md
theme:
  name: mkdocs
  locale: ru
  color_mode: dark
  user_color_mode_toggle: true
  custom_dir: custom/
```

- **docs/** - Папка с контентом сайта в формате Markdown, включая главную страницу `index.md`.

## Основные шаги выполнения проекта

1. **Создание кастомной темы**:
   - Создание папки `custom` и доработка шаблонов `base.html` и`content.html`.
   - Включение метаданных с названием сайта, описанием и автором.

2. **Настройка конфигурации MkDocs**:
   - Обновление `mkdocs.yml` для использования кастомной темы.

3. **Настройка пайплайна для GitHub Actions**:
   - Создание пайплайна в `.github/workflows/deploy.yml` для автоматической сборки и деплоя.
   - Установка Python, MkDocs и необходимых зависимостей.
   - Сборка сайта и деплой его на GitHub Pages.

4. **Дополнительная обработка** (опционально):
   - Валидация HTML и минификация с помощью `html-validator`, `autoprefixer` и `cssnano`.
   - Улучшение типографики с использованием JavaScript-библиотек.

## Как запустить проект

1. **Локальная сборка**:
   - Убедитесь, что установлены Python и MkDocs.
   - Выполните команду `mkdocs serve` для локального запуска сайта и проверки изменений.

2. **Автоматический деплой**:
   - Все изменения, запушенные в ветку `main`, автоматически собираются и деплоятся на GitHub Pages с помощью GitHub Actions.

## Результаты

Сайт успешно задеплоен и доступен по адресу на котором вы сейчас находитесь.
