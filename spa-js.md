# Создаем простое SPA на Vanilla JS

Одностраничное приложение (SPA) — это веб-приложение, которое загружает одну HTML-страницу и динамически обновляет контент без полной перезагрузки. В этой статье я покажу, как создать простое SPA на чистом JavaScript (Vanilla JS) с базовой маршрутизацией и динамическим контентом. Мы сделаем приложение с тремя страницами: "Главная", "О нас" и "Контакты".

## Что будем создавать

- **HTML-основа**: Минимальная структура с навигацией.
- **CSS**: Простые стили для адаптивного дизайна.
- **JavaScript**: Логика маршрутизации и обновления контента без перезагрузки страницы.
- **Функционал**: Переключение между страницами по клику на ссылки, используя хэш-маршрутизацию (`#`).

## Шаг 1: Настройка проекта

Создайте папку проекта с тремя файлами:

- `index.html`
- `styles.css`
- `app.js`

### HTML-структура

Создадим базовый HTML с навигацией и контейнером для контента.

```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Простое SPA на Vanilla JS</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <nav>
      <ul>
        <li><a href="#home">Главная</a></li>
        <li><a href="#about">О нас</a></li>
        <li><a href="#contacts">Контакты</a></li>
      </ul>
    </nav>
  </header>
  <main id="app">Загрузка...</main>
  <script src="app.js"></script>
</body>
</html>
```

- `<nav>` содержит ссылки с хэшами (`#home`, `#about`, `#contacts`) для маршрутизации.
- `<main id="app">` — контейнер, где будет отображаться контент страниц.

## Шаг 2: Стилизация

Добавим минимальные стили для адаптивного дизайна в `styles.css`.

```css
body {
  margin: 0;
  font-family: Arial, sans-serif;
  line-height: 1.5;
}

header {
  background: #333;
  color: white;
  padding: 1rem;
  text-align: center;
}

nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  justify-content: center;
  gap: 1rem;
}

nav a {
  color: white;
  text-decoration: none;
}

nav a:hover {
  text-decoration: underline;
}

main {
  max-width: 800px;
  margin: 2rem auto;
  padding: 0 1rem;
}

/* Мобильные стили */
@media (max-width: 600px) {
  nav ul {
    flex-direction: column;
    gap: 0.5rem;
  }
}
```

Эти стили создают простую навигацию и центрируют контент. Для мобильных устройств навигация становится вертикальной.

## Шаг 3: JavaScript для маршрутизации

В файле `app.js` реализуем маршрутизацию на основе хэша в URL и динамическое обновление контента.

```javascript
// Объект с маршрутами и их контентом
const routes = {
  '#home': `
    <h1>Главная</h1>
    <p>Добро пожаловать в наше SPA!</p>
  `,
  '#about': `
    <h1>О нас</h1>
    <p>Мы создаем крутые веб-приложения на JS.</p>
  `,
  '#contacts': `
    <h1>Контакты</h1>
    <p>Свяжитесь с нами: email@example.com</p>
  `,
  '': `
    <h1>Главная</h1>
    <p>Добро пожаловать в наше SPA!</p>
  ` // Дефолтная страница
};

// Функция рендеринга контента
function render() {
  const hash = window.location.hash || ''; // Получаем текущий хэш
  const content = routes[hash] || `
    <h1>404 - Страница не найдена</h1>
    <p>Проверьте URL или вернитесь на <a href="#home">главную</a>.</p>
  `; // Если маршрут не найден, показываем 404
  document.getElementById('app').innerHTML = content;
}

// Слушаем изменения хэша
window.addEventListener('hashchange', render);

// Рендерим при загрузке страницы
document.addEventListener('DOMContentLoaded', render);
```

### Как это работает:

1. **Маршруты**: Объект `routes` сопоставляет хэши (`#home`, `#about`) с HTML-контентом.
2. **Рендеринг**: Функция `render` читает текущий хэш из `window.location.hash` и обновляет содержимое `<main id="app">`.
3. **События**: Слушатель `hashchange` обновляет контент при смене хэша, а `DOMContentLoaded` рендерит страницу при загрузке.
4. **Обработка ошибок**: Если хэш не найден, отображается страница 404.

## Шаг 4: Тестирование

1. Сохраните файлы (`index.html`, `styles.css`, `app.js`) в одной папке.
2. Откройте `index.html` в браузере (например, через `file://` или локальный сервер, например, `npx serve`).
3. Щелкайте по ссылкам в навигации. Контент должен меняться без перезагрузки страницы.
4. Проверьте, что страница 404 отображается при неверном хэше (например, `#random`).
5. Используйте DevTools (`Ctrl + Shift + I`), чтобы протестировать мобильный вид и убедиться, что навигация адаптируется.

## Полезные советы

- **Локальный сервер**: Для тестирования используйте `npx serve` или VS Code Live Server.
- **Модульность**: Выделите контент страниц в отдельные функции или файлы, если проект разрастется.
- **История**: Для поддержки кнопки "назад" без хэшей изучите API `pushState`.

## Для продвинутых

1. **Добавьте данные через JSON**:

   ```javascript
   fetch('data.json')
     .then(response => response.json())
     .then(data => {
       routes['#home'] = `<h1>${data.title}</h1><p>${data.content}</p>`;
       render();
     });
   ```

2. **Анимации переходов**:

   ```css
   main {
     transition: opacity 0.3s;
   }
   ```

   ```javascript
   function render() {
     const app = document.getElementById('app');
     app.style.opacity = 0;
     setTimeout(() => {
       app.innerHTML = routes[window.location.hash] || '404';
       app.style.opacity = 1;
     }, 300);
   }
   ```

## Заключение

Мы создали простое SPA на Vanilla JS с хэш-маршрутизацией, адаптивным дизайном и динамическим контентом. Это базовый пример, который можно расширить: добавить загрузку данных, анимации или более сложную маршрутизацию. Попробуйте добавить новую страницу в `routes` или изменить стили в `styles.css`. Это отличный старт для понимания, как работают современные SPA-фреймворки, такие как React или Vue!