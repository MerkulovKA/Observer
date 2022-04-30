# Смена языка - компонент

![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/tailwindcss-%2338B2AC.svg?style=for-the-badge&logo=tailwind-css&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)

## Props
- Объект языков, содержащий объект с ключами:
    - Label - полное название языка
    - Code - краткое название языка
    
> Флаг языка подставляется по коду из папки 
> > site/assets/images/language/

*Пример входного объекта*

```JSON
{
    "ru": {
        "label": "Русский",
        "code": "ru"
    },
    "en": {
        "label": "English",
        "code": "en"
    }
}
```
*Итоговый код*
```html
<div class="drop-lang-container">
    <div class="drop-lang-label">
        <img src="site/assets/images/language/ru.svg" alt="ru">
        <span>
            Русский
        </span>
    </div>
    <div class="drop-lang-content">
        <a href="#" class="drop-lang-button hidden">
            <img src="site/assets/images/language/ru.svg" alt="ru">
            <span>
                RU
            </span>
        </a>
        <a href="#" class="drop-lang-button ">
            <img src="site/assets/images/language/en.svg" alt="en">
            <span>
                EN
            </span>
        </a>
    </div>
</div>
```
##Emit
Итогом работы будет смена языка
