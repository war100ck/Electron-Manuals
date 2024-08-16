# Оптимизация изображений

Этот документ описывает процесс сжатия изображений без потери качества с использованием библиотеки `sharp` и их перемещения в папку `dist` для оптимизации размера проекта.

## Содержание

1. [Установка](#установка)
2. [Сжатие изображений](#сжатие-изображений)
3. [Автоматизация процесса](#автоматизация-процесса)
4. [Запуск](#запуск)
5. [Заключение](#заключение)

## Установка

Следуйте этим шагам для установки и настройки инструментов для сжатия изображений:

1. Клонируйте репозиторий

    ```bash
    git clone https://github.com/yourusername/game-launcher.git
    cd game-launcher
    ```

2. Установите зависимости

    Убедитесь, что у вас установлены необходимые инструменты. Для сжатия изображений используйте библиотеку `sharp`:

    ```bash
    npm install sharp
    ```

## Сжатие изображений

1. Создайте папку `dist`

    Перед началом работы создайте папку `dist`, если она не существует:

    ```bash
    mkdir -p dist
    ```

2. Создайте скрипт для сжатия изображений

    В корневой директории проекта создайте файл `compressImages.js` и добавьте следующий код:

    ```javascript
    const fs = require('fs');
    const path = require('path');
    const sharp = require('sharp');

    // Папки для входных и выходных изображений
    const inputDir = path.join(__dirname, 'assets');
    const outputDir = path.join(__dirname, 'dist');

    // Функция для обработки изображений
    async function compressImages() {
      // Чтение файлов из папки assets
      fs.readdir(inputDir, (err, files) => {
        if (err) {
          console.error('Ошибка чтения папки:', err);
          return;
        }

        // Обработка каждого файла
        files.forEach(async (file) => {
          const filePath = path.join(inputDir, file);
          const outputFilePath = path.join(outputDir, file);

          // Проверка расширения файла
          if (['.png', '.jpg', '.jpeg'].includes(path.extname(file).toLowerCase())) {
            try {
              // Чтение и сжатие изображения
              const image = sharp(filePath);
              const metadata = await image.metadata();

              // Сжатие изображения
              await image
                .toBuffer()  // Преобразование в буфер
                .then(data => {
                  sharp(data)
                    .toFile(outputFilePath, (err, info) => {
                      if (err) {
                        console.error(`Ошибка при сохранении файла ${file}:`, err);
                      } else {
                        console.log(`Изображение сохранено: ${outputFilePath}`);
                      }
                    });
                })
                .catch(err => console.error(`Ошибка обработки файла ${file}:`, err));
            } catch (err) {
              console.error(`Ошибка обработки файла ${file}:`, err);
            }
          }
        });
      });
    }

    // Создание папки dist, если она не существует
    if (!fs.existsSync(outputDir)) {
      fs.mkdirSync(outputDir);
    }

    // Запуск функции сжатия
    compressImages();
    ```

3. Обновите пути в HTML-файлах

    Если вы используете изображения в ваших HTML-файлах, убедитесь, что пути к изображениям указывают на сжатые версии в папке `dist`.

## Автоматизация процесса

Чтобы упростить процесс сжатия изображений, добавьте следующий скрипт в раздел `scripts` вашего `package.json`:

```json
"scripts": {
  "compress-images": "node compressImages.js"
}
``` 
3. Запуск

    Теперь вы можете запускать процесс сжатия изображений с помощью команды:

	```bash
    npm run compress-images
    ```	
   Этот файл `README.md` включает инструкции по установке, созданию скрипта для сжатия изображений, автоматизации процесса и запуску скрипта. Вы можете адаптировать его в зависимости от особенностей вашего проекта.

3. Заключение

   Теперь ваши изображения сжаты и размещены в папке dist, что помогает уменьшить размер проекта и улучшить производительность.

