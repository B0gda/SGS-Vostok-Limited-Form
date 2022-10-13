# Web-form on Vue.js

## Form
![form](https://github.com/B0gda/SGS-Vostok-Limited-Form/blob/main/src/assets/screenshotOfForm.png) 
---


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

---
## About
1. Связи между городами-цехами-сотрудниками:
    
    Для искусственного разделения по "направлениям", 3 группы были разбиты следующим образом:
    * Первые 2 пукта из города <-> первый  цех <-> первые 2 типа сотрудника.
    * Последующие 2 города <->  второй цех <-> третий тип сотрудника.
    * Последние 2 города <->  третий цех <-> все оставшиеся типы сотрудников.
2. Связи между смены-бригады:
    * Первые три бригады связаны с первой сменой(с 8:00 до 20:00), а 4 и 5 бригады связаны с второй сменой(с 20:00 до 8:00).

