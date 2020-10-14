# hotels_test_app

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

###Task
Внешний вид значения не имеет, реализовать полностью на фронте на react/vue (vue приоритетнее), разрешается и поощряется использование готовых компонентов (к примеру, react-select).

Фильтр "Страна" - выпадающий список с поиском
Фильтр "Тип" - выпадающий список с множественным выбором
Фильтр "Звезды" - группа чекбоксов
Фильтр "Количество отзывов" - поле для ввода количества, допустимы только целые числа
Фильтр "Цена до" - слайдер, при наведении и клике на ползунок под ним отображается установленное значение

По нажатию кнопки "Применить фильтры" список в правой части должен отфильтровываться в соотвествии с установленными значениями фильтров.
По нажатию кнопки "Очистить фильтры" все фильтры должны устанавливаться в начальное состояние.
Максимальное выводимое количество записей на странице - 3 штуки.
Если по установленным условиям фильтров записи не найдены вместо таблицы отображается текст "Записей не найдено".
