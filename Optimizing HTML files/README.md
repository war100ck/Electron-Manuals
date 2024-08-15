# Оптимизация HTML-файлов

Этот документ описывает процесс сжатия HTML-файлов и их переноса в папку `dist` для оптимизации размера проекта.

## Содержание

1. [Установка](#установка)
2. [Сжатие HTML-файлов](#сжатие-html-файлов)
3. [Автоматизация процесса](#автоматизация-процесса)
4. [Запуск](#запуск)

## Установка

Следуйте этим шагам для установки и настройки инструментов для сжатия HTML-файлов:

1. Клонируйте репозиторий

    ```bash
    git clone https://github.com/yourusername/game-launcher.git
    cd game-launcher
    ```

2. Установите зависимости

    Убедитесь, что у вас установлены необходимые инструменты. Для сжатия HTML используйте `html-minifier-terser`, а для создания папок через командную строку используйте `mkdirp`:

    ```bash
    npm install -g html-minifier-terser mkdirp
    ```

## Сжатие HTML-файлов

1. Создайте папку `dist`

    Перед началом работы создайте папку `dist`, если она не существует:

    ```bash
    mkdirp dist
    ```

2. Сожмите HTML файлы и переместите их в папку `dist`

    Выполните следующую команду для сжатия HTML файлов и сохранения их в папке `dist`:

    ```bash
    html-minifier-terser --input-dir . --output-dir dist --file-ext html --collapse-whitespace --remove-comments --minify-js true --minify-css true
    ```

3. Обновите пути в `index.html`

    Измените пути к HTML-файлам в вашем проекте, чтобы они указывали на сжатые версии в папке `dist`. Например:

    ```html
    <link rel="stylesheet" href="dist/styles.min.css">
    <link rel="stylesheet" href="dist/settings.min.css">
    ```

## Автоматизация процесса

Чтобы упростить процесс сжатия и перемещения файлов, добавьте следующий скрипт в раздел `scripts` вашего `package.json`:

```json
"scripts": {
  "build-html": "mkdirp dist && html-minifier-terser --input-dir . --output-dir dist --file-ext html --collapse-whitespace --remove-comments --minify-js true --minify-css true"
}
