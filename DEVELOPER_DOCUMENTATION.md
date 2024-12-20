# Документация разработчика

## Текст программы

### Наименование программы

**ImageProcessingApp** — веб-приложение для обработки изображений с использованием библиотеки OpenCV.js.

---

### Область применения

Программа предназначена для выполнения базовых операций обработки изображений в браузере. Она может быть использована:
- В учебных целях для изучения алгоритмов обработки изображений.
- Для демонстрации возможностей библиотеки OpenCV.js.
- В качестве основы для создания более сложных приложений обработки изображений.

---

### Назначение программы

- Реализация и визуализация следующих операций обработки изображений:
  - Выравнивание гистограммы (Equalize Histogram).
  - Изменение линейного контраста (Linear Contrast).
  - Повышение резкости изображения (Sharpen).
- Предоставление простого и удобного графического интерфейса пользователя (GUI) для работы с изображениями.
- Загрузка изображений с локального устройства и их обработка в реальном времени.

---

### Функциональные возможности

- Загрузка изображений через интерфейс браузера.
- Отображение загруженного изображения на холсте `<canvas>`.
- Применение различных операций обработки изображений:
  - Выравнивание гистограммы.
  - Изменение контраста.
  - Увеличение резкости.
- Визуализация результата обработки непосредственно в браузере.

---

## Описание программы

### Структура программы

Программа состоит из следующих разделов:

1. **HTML**:
   - Элементы интерфейса: кнопки, область для загрузки изображения и холст `<canvas>`.
2. **CSS**:
   - Оформление интерфейса: центрирование элементов, стилизация кнопок и холста.
3. **JavaScript**:
   - Логика обработки изображений с использованием библиотеки OpenCV.js.
   - Реализация функций для выполнения операций обработки изображений.

---

### Используемые библиотеки и модули

- **OpenCV.js**: библиотека для обработки изображений в браузере.
  - Используется для выполнения операций, таких как преобразование цветовой модели, применение фильтров, изменение контраста и резкости.
  - Подключается через CDN:
    ```html
    <script async src="https://docs.opencv.org/4.x/opencv.js"></script>
    ```

---

### Логические структуры данных

- **Переменные и элементы интерфейса**:
  - `upload`: элемент `<input>` для загрузки изображений.
  - `canvas`: элемент `<canvas>` для отображения и обработки изображений.
  - `ctx`: контекст рендеринга холста для работы с изображениями.
  - `originalImage`: переменная для хранения исходного изображения (объект `ImageData`).
- **Функции обработки изображений**:
  - `equalizeHistogram`: выравнивание гистограммы.
  - `linearContrast`: изменение контраста.
  - `sharpenImage`: повышение резкости.

---

### Взаимодействие с пользователем

1. Пользователь загружает изображение через элемент `<input type="file">`.
2. Загруженное изображение отображается на холсте `<canvas>`.
3. Пользователь нажимает одну из кнопок для выполнения операции обработки изображения:
   - "Equalize Histogram" — выравнивание гистограммы.
   - "Linear Contrast" — изменение контраста.
   - "Sharpen" — повышение резкости.
4. Обработанное изображение отображается на том же холсте.

---

## Инструкция по установке и запуску

### Требования к системе

- Любой современный браузер с поддержкой JavaScript и HTML5.
- Подключение к интернету для загрузки библиотеки OpenCV.js.

---

### Установка

1. Скопируйте исходный код программы и сохраните его в файл с расширением `.html`, например, `image_processing_app.html`.
2. Убедитесь, что устройство подключено к интернету (для загрузки OpenCV.js).

---

### Запуск программы

1. Откройте файл `image_processing_app.html` в любом современном браузере (Chrome, Firefox, Edge и т.д.).
2. Приложение загрузится, и вы увидите интерфейс с кнопками и областью для загрузки изображения.

---

## Инструкция пользователя

1. Нажмите кнопку "Выбрать файл" и загрузите изображение с вашего устройства.
2. После загрузки изображение отобразится на холсте.
3. Выберите одну из операций обработки изображения:
   - "Equalize Histogram": выравнивание гистограммы для улучшения контрастности.
   - "Linear Contrast": увеличение контраста изображения.
   - "Sharpen": повышение резкости изображения.
4. Обработанное изображение будет отображено на том же холсте.
5. Чтобы обработать другое изображение, загрузите новый файл.

---

## Требования к техническим характеристикам

- **Процессор**: с тактовой частотой не менее 1 ГГц.
- **Оперативная память**: не менее 512 МБ.
- **Браузер**: современный браузер с поддержкой HTML5 и JavaScript.

---

## Описание функций

### 1. Загрузка изображения
```javascript
upload.addEventListener('change', function(event) {
    const file = event.target.files[0];
    const reader = new FileReader();
    reader.onload = function(e) {
        const img = new Image();
        img.onload = function() {
            canvas.width = img.width;
            canvas.height = img.height;
            ctx.drawImage(img, 0, 0);
            originalImage = ctx.getImageData(0, 0, img.width, img.height);
        };
        img.src = e.target.result;
    };
    reader.readAsDataURL(file);
});
```
- Загружает изображение и отображает его на холсте `<canvas>`.

---

### 2. Выравнивание гистограммы
```javascript
function equalizeHistogram() {
    if (!originalImage) return;
    let src = cv.imread(canvas);
    let dst = new cv.Mat();
    cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
    cv.equalizeHist(src, dst);
    cv.imshow(canvas, dst);
    src.delete(); dst.delete();
}
```
- Преобразует изображение в оттенки серого.
- Применяет выравнивание гистограммы для улучшения контрастности.

---

### 3. Линейный контраст
```javascript
function linearContrast() {
    if (!originalImage) return;
    let src = cv.imread(canvas);
    let dst = new cv.Mat();
    src.convertTo(dst, -1, 1.2, 0); // Увеличение контраста
    cv.imshow(canvas, dst);
    src.delete(); dst.delete();
}
```
- Изменяет контраст изображения с помощью коэффициента усиления (в данном случае 1.2).

---

### 4. Повышение резкости
```javascript
function sharpenImage() {
    if (!originalImage) return;
    let src = cv.imread(canvas);
    let dst = new cv.Mat();
    let kernel = cv.Mat.eye(3, 3, cv.CV_32F);
    kernel.data32F[4] = -8.0; // Центр ядра
    cv.filter2D(src, dst, cv.CV_8U, kernel);
    cv.addWeighted(src, 1.5, dst, -0.5, 0, dst);
    cv.imshow(canvas, dst);
    src.delete(); dst.delete(); kernel.delete();
}
```
- Применяет фильтр для повышения резкости изображения.

---

## Обработка ошибок

- Если изображение не загружено, обработка не начнется.
- Если файл имеет неподдерживаемый формат, изображение не будет загружено.

---

## Сопровождение и развитие

- Код программы легко расширяется для добавления новых операций обработки изображений.
- Для добавления новой функции:
  1. Реализуйте функцию обработки изображения с использованием OpenCV.js.
  2. Добавьте соответствующую кнопку в HTML.
  3. Свяжите кнопку с новой функцией.

---

## Заключение

Программа представляет собой удобный инструмент для базовой обработки изображений в браузере. Она может быть использована как в учебных, так и в демонстрационных целях. Код хорошо структурирован и легко расширяется, что делает его подходящим для дальнейшего развития.