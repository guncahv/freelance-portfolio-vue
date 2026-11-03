# aykhan.dev — портфолио фриланс-разработчика (Vue.js)

Лабораторная работа №21 «Построение интерфейса веб-приложения на Vue.js с использованием экземпляра Vue, компонентов и директив».
Вариант 1 — «Портфолио фриланс-разработчика».

Выполнил: Рахманкулов А., группа 310101.

## Стек

- Vue 3 (Options API: `data()`, `props`, `computed`, `methods`, `$emit`)
- Vite
- Однофайловые компоненты `.vue` со стилями `<style scoped>`; дизайн-токены ЛР №17 в `src/style.css`

## Запуск проекта

Требуется установленный [Node.js](https://nodejs.org/) (версия 20.19 или новее).

```bash
git clone https://github.com/guncahv/freelance-portfolio-vue.git
cd freelance-portfolio-vue
npm install
npm run dev
```

После запуска откройте в браузере адрес из терминала (обычно http://localhost:5173).

Сборка production-версии:

```bash
npm run build
npm run preview
```

## Структура

```
src/
├── assets/
│   ├── icons/            # логотип и SVG-иконки из ЛР №17
│   └── images/           # фото специалиста, обложки проектов
├── components/
│   ├── FilterInput.vue   # поле поиска (v-model)
│   ├── ProjectCard.vue   # карточка проекта
│   └── ProjectList.vue   # список карточек (v-for)
├── App.vue               # корневой компонент: данные, фильтрация (computed)
├── main.js               # создание экземпляра Vue
└── style.css             # глобальные стили и дизайн-токены
```

## Компоненты

| Компонент | Props | Реактивные данные `data()` | События `$emit` |
|---|---|---|---|
| `App` | — | `projects`, `searchQuery`, `onlyFavorites`, `favoriteIds`, `navItems`, `activeNav` | — |
| `FilterInput` | `modelValue: String` (обяз.), `label`, `placeholder` | `query`, `isFocused` | `update:modelValue` |
| `ProjectList` | `projects: Array`, `total: Number`, `favoriteIds: Array` (обяз.), `query: String` | `layout` | `toggle-favorite`, `reset-filter` |
| `ProjectCard` | `project: Object` (обяз.), `isFavorite: Boolean` | `isExpanded` | `toggle-favorite` |

Фильтрация выполняется в `App.vue` вычисляемым свойством `filteredProjects` по тексту из `FilterInput` (`v-model="searchQuery"`) и флажку «Только избранные» (`v-model="onlyFavorites"`).
