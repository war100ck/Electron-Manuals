# Минификация JavaScript-файлов с помощью Terser

Terser — это популярный инструмент для минимизации и обфускации JavaScript-кода. В этом руководстве описаны шаги по использованию Terser для сжатия JavaScript-файлов в вашем проекте.

## Содержание

1. [Установка](#установка)
2. [Минификация JavaScript-файлов](#минификация-javascript-файлов)
3. [Запуск приложения](#запуск-приложения)

## Установка

Следуйте этим шагам для установки и настройки проекта:

1. Клонируйте репозиторий

    ```bash
    git clone https://github.com/yourusername/game-launcher.git
    cd game-launcher
    ```

2. Установите зависимости

    ```bash
    npm install
    ```

## Минификация JavaScript-файлов

Для уменьшения размера JavaScript-файлов используйте Terser.

1. Установка Terser

    Установите Terser как зависимость для вашего проекта:

    ```bash
    npm install terser --save-dev
    ```

2. Создание конфигурационного файла Terser (опционально)

    Создайте файл `terser.config.json` в корне вашего проекта:

    ```json
    {
      "compress": {
        "drop_console": true
      },
      "mangle": {
        "toplevel": true
      },
      "output": {
        "comments": false
      }
    }
    ```

    **Описание конфигурации:**

    - `compress`: Убирает `console.log` и другие команды отладки.
    - `mangle`: Обфускация имен переменных.
    - `output.comments`: Убирает все комментарии.

3. Минификация файлов JavaScript с помощью Terser

    1. Минификация отдельных файлов

        Используйте следующие команды для минификации каждого файла:

        ```bash
        npx terser main.js --config-file terser.config.json --output dist/main.min.js
        npx terser preload.js --config-file terser.config.json --output dist/preload.min.js
        npx terser renderer.js --config-file terser.config.json --output dist/renderer.min.js
        npx terser settingsRenderer.js --config-file terser.config.json --output dist/settingsRenderer.min.js
        ```

    2. Минификация всех файлов с помощью скрипта

        Добавьте следующий скрипт в `package.json`:

        ```json
        "scripts": {
          "minify-js": "npx terser main.js --config-file terser.config.json --output dist/main.min.js && npx terser preload.js --config-file terser.config.json --output dist/preload.min.js && npx terser renderer.js --config-file terser.config.json --output dist/renderer.min.js && npx terser settingsRenderer.js --config-file terser.config.json --output dist/settingsRenderer.min.js"
        }
        ```

        Запустите скрипт командой:

        ```bash
        npm run minify-js
        ```

4. Замена оригинальных файлов на минифицированные

    После минификации замените ссылки на скрипты в `index.html` на минифицированные версии. Например:

    ```html
    <script src="dist/main.min.js"></script>
    <script src="dist/preload.min.js"></script>
    <script src="dist/renderer.min.js"></script>
    <script src="dist/settingsRenderer.min.js"></script>
    ```

5. Удаление исходных файлов (опционально)

    Если вам не нужны исходные JavaScript-файлы после минификации, вы можете удалить их:

    ```bash
    rm main.js preload.js renderer.js settingsRenderer.js
    ```

6. Проверка работы приложения

    После того как вы заменили скрипты, протестируйте приложение, чтобы убедиться, что оно работает корректно с минифицированными файлами.

## Запуск приложения

1. Запуск приложения в режиме разработки

    Для запуска приложения в режиме разработки используйте команду:

    ```bash
    npm start
    ```

2. Сборка и запуск приложения

    Для сборки и запуска приложения в продуктивном режиме используйте Electron Packager или другой инструмент сборки, указав папку `dist` как место для вывода минифицированных файлов.

    ```bash
    npx electron-packager . --out=build --overwrite
    ```

## Заключение

Теперь ваш проект готов к использованию и оптимизирован для продакшена. Если у вас есть вопросы или предложения, не стесняйтесь открывать issue на GitHub.
