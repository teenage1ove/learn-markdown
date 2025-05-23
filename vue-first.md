# Введение во Vue: Первый компонент

Vue.js — это легковесный и гибкий JavaScript-фреймворк для создания интерактивных пользовательских интерфейсов. В этой статье я покажу, как создать первый компонент Vue 3, используя **Composition API**. Наш компонент будет счетчиком, который увеличивается и уменьшается по клику на кнопки.

## Шаг 1: Настройка проекта

Для работы с `.vue` файлами рекомендуется использовать сборщик, например Vite, который поддерживает однофайловые компоненты. Мы создадим проект с Vite и добавим компонент счетчика.

### Установка Vite

1. Создайте проект с помощью команды:

   ```bash
   npm create vite@latest my-vue-counter -- --template vue
   ```

2. Перейдите в папку проекта и установите зависимости:

   ```bash
   cd my-vue-counter
   npm install
   ```

3. Запустите локальный сервер для разработки:

   ```bash
   npm run dev
   ```

Vite создаст проект с базовой структурой, включая файл `src/App.vue`.

## Шаг 2: Создание компонента счетчика

Заменим содержимое `src/App.vue` на наш компонент счетчика. В `.vue` файле есть три секции: `<template>` для HTML, `<script setup>` для JavaScript и `<style>` для CSS.

### Содержимое `src/App.vue`

```vue
<template>
  <div class="container">
    <h1>Счетчик: {{ count }}</h1>
    <button @click="increment">Увеличить</button>
    <button @click="decrement">Уменьшить</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Реактивная переменная для счетчика
const count = ref(0);

// Функции для изменения счетчика
const increment = () => {
  count.value++;
};
const decrement = () => {
  count.value--;
};
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 2rem auto;
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  text-align: center;
}

h1 {
  color: #333;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  margin: 0.5rem;
  border: none;
  border-radius: 4px;
  background: #42b983;
  color: white;
  cursor: pointer;
}

button:hover {
  background: #369874;
}
</style>
```

### Как это работает:

1. **`<template>`**: Содержит HTML-структуру с:
   - Заголовком, отображающим значение `count` через интерполяцию `{{ count }}`.
   - Двумя кнопками с директивами `@click` для вызова функций `increment` и `decrement`.
2. **`<script setup>`**:
   - Импортируем `ref` из Vue для создания реактивной переменной.
   - Объявляем `count` как `ref(0)` — реактивное значение счетчика.
   - Определяем функции `increment` и `decrement`, которые изменяют `count.value`.
   - Все переменные и функции автоматически доступны в шаблоне благодаря `<script setup>`.
3. **`<style scoped>`**: Стили применяются только к этому компоненту (`scoped`).

## Шаг 3: Тестирование

1. Запустите проект с помощью `npm run dev`.
2. Откройте браузер по адресу, указанному Vite (обычно `http://localhost:5173`).
3. Нажмите кнопки "Увеличить" и "Уменьшить" — счетчик должен обновляться в реальном времени.
4. Используйте DevTools (`Ctrl + Shift + I`) для проверки DOM и отладки JavaScript.

## Шаг 4: Интеграция в проект

Чтобы использовать наш компонент в другом `.vue` файле, импортируйте его. Например, создайте `src/components/Counter.vue` с тем же кодом и подключите в `src/App.vue`:

```vue
<template>
  <div>
    <h1>Мое приложение</h1>
    <Counter />
  </div>
</template>

<script setup>
import Counter from './components/Counter.vue';
</script>
```

## Полезные советы

- **Реактивность с `ref`**: Используйте `ref` для простых значений (числа, строки). Для объектов используйте `reactive`:

  ```javascript
  import { reactive } from 'vue';
  const state = reactive({ count: 0 });
  ```

- **Composables**: Для переиспользуемой логики создайте composable:

  ```javascript
  // src/composables/useCounter.js
  import { ref } from 'vue';

  export function useCounter() {
    const count = ref(0);
    const increment = () => count.value++;
    const decrement = () => count.value--;
    return { count, increment, decrement };
  }
  ```

  Использование в компоненте:

  ```vue
  <script setup>
  import { useCounter } from './composables/useCounter';
  const { count, increment, decrement } = useCounter();
  </script>
  ```

- **Директивы Vue**: Изучите `v-if`, `v-for`, `v-bind` для условного рендеринга, циклов и привязки атрибутов.
- **Сброс счетчика**: Добавьте кнопку для сброса:

  ```vue
  <template>
    <div class="container">
      <h1>Счетчик: {{ count }}</h1>
      <button @click="increment">Увеличить</button>
      <button @click="decrement">Уменьшить</button>
      <button @click="reset">Сбросить</button>
    </div>
  </template>

  <script setup>
  import { ref } from 'vue';
  const count = ref(0);
  const increment = () => count.value++;
  const decrement = () => count.value--;
  const reset = () => count.value = 0;
  </script>
  ```

- **Сборка проекта**: Для продакшена соберите проект с помощью:

  ```bash
  npm run build
  ```

## Заключение

Мы создали первый компонент Vue с использованием Composition API. Счетчик демонстрирует реактивность и простоту синтаксиса Vue 3. Попробуйте добавить новые функции, например, ограничение счетчика (чтобы не уходил в отрицательные значения) или сохранение значения в `localStorage`.
