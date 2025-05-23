# Принципы KISS, DRY, YAGNI, BDUF, SOLID, APO и бритва Оккама в JavaScript

Эти принципы помогают писать чистый, поддерживаемый и эффективный код. Ниже — краткое описание каждого принципа с примерами применения в JavaScript.

## 1. KISS (Keep It Simple, Stupid)

**Суть**: Делай код максимально простым и понятным, избегай ненужной сложности.

**Пример**:

```javascript
// Плохо: Сложная функция с избыточной логикой
function formatUserGreeting(user) {
  return user && user.name ? `Hello, ${user.name.toUpperCase()}!` : 'Hello, Guest!';
}

// Хорошо: Простое решение
function greet(name = 'Guest') {
  return `Hello, ${name}!`;
}
console.log(greet('Alice')); // Hello, Alice!
console.log(greet()); // Hello, Guest!
```

**Почему лучше**: Простая функция с параметром по умолчанию читается легче и делает то же самое.

## 2. DRY (Don't Repeat Yourself)

**Суть**: Избегай дублирования кода, выноси повторяющуюся логику в функции или модули.

**Пример**:

```javascript
// Плохо: Дублирование форматирования
const user1 = { name: 'Alice', age: 25 };
const user2 = { name: 'Bob', age: 30 };
console.log(`${user1.name} is ${user1.age} years old`);
console.log(`${user2.name} is ${user2.age} years old`);

// Хорошо: Функция для форматирования
function formatUser(user) {
  return `${user.name} is ${user.age} years old`;
}
console.log(formatUser(user1)); // Alice is 25 years old
console.log(formatUser(user2)); // Bob is 30 years old
```

**Почему лучше**: Одна функция вместо повторяющегося кода упрощает поддержку и изменения.

## 3. YAGNI (You Aren't Gonna Need It)

**Суть**: Не добавляй функциональность, пока она реально не нужна.

**Пример**:

```javascript
// Плохо: Избыточная логика "на будущее"
function calculatePrice(item) {
  let discount = 0;
  if (item.type === 'premium') discount = 0.2; // Будущий функционал, пока не нужен
  return item.price * (1 - discount);
}

// Хорошо: Только текущая потребность
function calculatePrice(item) {
  return item.price;
}
console.log(calculatePrice({ price: 100 })); // 100
```

**Почему лучше**: Избегаем ненужного кода, который может никогда не понадобиться.

## 4. BDUF (Big Design Up Front)

**Суть**: Тщательное планирование архитектуры перед началом написания кода.

**Пример**:

```javascript
// Плохо: Писать код без структуры для SPA
// (куча функций в одном файле без маршрутизации)

// Хорошо: Планируем модули для SPA
// Файл: router.js
const routes = {
  '/home': () => '<h1>Home</h1>',
  '/about': () => '<h1>About</h1>'
};
function renderRoute(path) {
  document.getElementById('app').innerHTML = routes[path] || '<h1>404</h1>';
}

// Файл: main.js
renderRoute(window.location.pathname);
```

**Почему лучше**: Планирование модулей (маршрутизация, компоненты) упрощает масштабирование.

## 5. SOLID

**S**ingle Responsibility, **O**pen/Closed, **L**iskov Substitution, **I**nterface Segregation, **D**ependency Inversion.

### S: Принцип единственной ответственности

**Суть**: Каждый модуль или функция отвечает за одну задачу.

**Пример**:

```javascript
// Плохо: Функция делает все
function processUser(user) {
  const greeting = `Hello, ${user.name}`;
  console.log(greeting);
  saveToDatabase(user);
}

// Хорошо: Разделяем ответственность
function createGreeting(user) {
  return `Hello, ${user.name}`;
}
function logGreeting(greeting) {
  console.log(greeting);
}
logGreeting(createGreeting({ name: 'Alice' })); // Hello, Alice!
```

**Почему лучше**: Легче тестировать и поддерживать.

### O: Принцип открытости/закрытости

**Суть**: Код открыт для расширения, но закрыт для модификации.

**Пример**:

```javascript
// Плохо: Изменяем функцию для нового формата
function formatData(data) {
  return JSON.stringify(data); // Нужно добавить XML? Придется менять
}

// Хорошо: Используем объект, который легко расширять
const formatters = {
  json: (data) => JSON.stringify(data),
  xml: (data) => `<data>${data.value}</data>`
};
function formatData(data, type = 'json') {
  return formatters[type](data);
}
console.log(formatData({ value: 'test' }, 'json')); // {"value":"test"}
```

**Почему лучше**: Новый формат добавляется без изменения функции.

### L: Принцип подстановки Лисков

**Суть**: Подклассы могут заменять базовые без изменения поведения.

**Пример**:

```javascript
// Плохо: Подкласс нарушает контракт
class Button {
  click() {
    return 'Button clicked';
  }
}
class ImageButton extends Button {
  click() {
    return null; // Ожидается строка, но возвращается null
  }

// Хорошо: Соблюдаем контракт
class Button {
  click() {
    return 'Button clicked';
  }
}
class IconButton extends Button {
  click() {
    return 'IconButton clicked'; // Возвращает строку, как ожидается
  }
}
const buttons = [new Button(), new IconButton()];
buttons.forEach(btn => console.log(btn.click())); // Button clicked, IconButton clicked
```

**Почему лучше**: `IconButton` возвращает строку, как ожидает базовый класс `Button`, что позволяет использовать его в любом контексте, где ожидается `Button`.

### I: Принцип разделения интерфейсов

**Суть**: Не заставляй использовать ненужные методы.

**Пример**:

```javascript
// Плохо: Общий интерфейс с лишними методами
class BigComponent {
  render() {}
  save() {} // Не всем компонентам нужно сохранять
}

// Хорошо: Разделяем интерфейсы
class Renderable {
  render() {
    return '<div>Content</div>';
  }
}
class Savable {
  save() {
    console.log('Saved');
  }
}
class SmallComponent extends Renderable {}
```

**Почему лучше**: Компонент использует только нужные методы.

### D: Принцип инверсии зависимостей

**Суть**: Зависимости должны быть абстрактными, а не конкретными.

**Пример**:

```javascript
// Плохо: Прямая зависимость от fetch
function getUsers() {
  return fetch('/api/users').then(res => res.json());
}

// Хорошо: Инверсия через параметр
async function getUsers(fetchFn = fetch) {
  return fetchFn('/api/users').then(res => res.json());
}
```

**Почему лучше**: Легко подменить `fetch` для тестов (например, mock).

## 6. APO (Avoid Premature Optimization)

**Суть**: Не оптимизируй код раньше времени.

**Пример**:

```javascript
// Плохо: Преждевременная оптимизация фильтрации
function filterActiveUsers(users) {
  const active = [];
  for (let i = 0, len = users.length; i < len; i++) {
    if (users[i].isActive) active.push(users[i]); // Кэшируем длину и используем цикл
  }
  return active;
}

// Хорошо: Простое и читаемое решение
function filterActiveUsers(users) {
  return users.filter(user => user.isActive);
}
const users = [
  { name: 'Alice', isActive: true },
  { name: 'Bob', isActive: false }
];
console.log(filterActiveUsers(users)); // [{ name: 'Alice', isActive: true }]
```

**Почему лучше**: Метод `filter` проще, читаемее и достаточно быстр для большинства случаев. Оптимизация цикла не нужна, пока нет доказанных проблем с производительностью.

## 7. Бритва Оккама

**Суть**: Выбирай самое простое решение, если оно решает задачу.

**Пример**:

```javascript
// Плохо: Сложная проверка
function isAdult(user) {
  return user && user.age && typeof user.age === 'number' && user.age >= 18;
}

// Хорошо: Простое решение
function isAdult(age) {
  return age >= 18;
}
console.log(isAdult(20)); // true
```

**Почему лучше**: Минимальная проверка решает задачу без лишней логики.
