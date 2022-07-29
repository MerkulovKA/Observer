# Picture и&nbsp;с&nbsp;чем его едят

<details>
<summary>Определение ...</summary>
HTML-элемент<kbd>picture</kbd>служит контейнером для одного или более элементов<kbd>source</kbd>и&nbsp;одного элемента<kbd>img</kbd>для обеспечения оптимальной версии изображения для различных размеров экрана. Браузер рассмотрит каждый из&nbsp;дочерних элементов<kbd>source</kbd>и&nbsp;выберет один, соответствующий лучшему совпадению; если совпадений среди элементов<kbd>source</kbd>найдено не&nbsp;будет, то&nbsp;будет выбран файл, указанный атрибутом<kbd>src</kbd>элемента<kbd>img</kbd>. Затем выбранное изображение отображается в&nbsp;пространстве, занятом элементом <kbd>img</kbd>
</details>

Чтобы выбрать оптимальное изображение, **`Браузер`** анализирует атрибуты `srcset`, `media`, и `type` элемента <kbd>source</kbd> и выбирает совместимое изображение, которое наилучшим образом соответствует текущему макету страницы, характеристикам устройства отображения и т.д.

Магии в этом теги много, но мы попробуем разобраться.


## Структура тега
```html
    <picture>
        <source srcset="images/400.webp" type="image/webp">
        <source srcset="images/400.png" type="image/png">
        <img src="images/400.jpg"
             width="400"
             height="114"
             alt="alt">
    </picture>
```
В вышеприведенном примере, мы имеем тег `picture`, который содержит два элемента `source` и один `img`
> ### Важно отметить:
> 1. Тег `img` является обязательным
> 2. Атрибут `type` является обязательным для корректной обработки тега `picture`
> 3. Выбранный вариант изображения будет брать стили, установленные на тег `img`
> 4. Хоть тег `picture` и умный, при первом совпадении размера (ширины) картинки он остановится. Т.е. если в вышеприведенном примере мы поменяем местами первый и второй `source`, он оставит `.png`, хоть `.webp` и лучше, как по качеству, так и по размеру файла

## Управление качеством

У мониторов есть свойство `devicePixelRatio`, которое содержит отношение разрешения дисплея текущего устройства в физических пикселях к разрешению в логических (CSS) пикселях. Также это значение можно интерпретировать как отношение размера одного физического пикселя к размеру одного логического (CSS) пикселя.

И если мы захотим в зависимости от плотности пикселей экрана показывать разные картинки (к примеру, показать более качественную), то нам нужно в элементе `source` указать какие картинки выводить в зависимости от плотности

```html
<picture>
        <source srcset="images/400.webp 1x, 
                        images/1000.png 1.5x, 
                        images/1400.webp 2x"
                type="image/webp">
        <source
                srcset="images/400.png 1x, 
                        images/1000.png 1.5x, 
                        images/1400.png 2x"
                type="image/png">
        <img src="images/400.jpg"
             alt="">
    </picture>
```
Так, при рендере страницы, браузер отобразит ту картинку, которая подходит к вашему экрану относительно плотности пикселей.

Ширина экрана никак не влияет на данный сценарий.

### !!При изменении ширины браузера картинка остается!!
> Проверить `devicePixelRatio` вы можете открыв консоль в браузере и написав <kbd>window.devicePixelRatio</kbd>

## "Ручная" коробка
В принципе, мы можем сами определить, когда и какую картинку вывести воспользовавшись атрибутом `media`
```html
    <picture>
        <source media="(max-width: 400px)" srcset="images/400.webp" type="image/webp">
        <source media="(max-width: 1000px)" srcset="images/1000.webp" type="image/webp">
        <source media="(max-width: 1400px)" srcset="images/1400.webp" type="image/webp">
        <source media="(max-width: 400px)" srcset="images/400.png" type="image/png">
        <source media="(max-width: 1000px)" srcset="images/1000.png" type="image/png">
        <source media="(max-width: 1400px)" srcset="images/1400.png" type="image/png">
        <img
                src="images/400.jpg"
                alt="">
    </picture>
```
Единственное что остается тегу, определить поддерживает ли браузер расширение `.webp` и вывести подходящее расширение
### !!При изменении ширины браузера картинка отображаются в зависимости от размера экрана!!

## Коробка "Автомат"
Зачем нам самим прописывать медиа-запросы, если мы можем всё скинуть на браузер?

А нужно нам для этого совсем немного - указать в атрибуте `srcset` ссылки на картинки и их фактическую ширину через <kbd>,</kbd>
```html
    <picture>
        <source srcset="images/400.webp 400w,
                        images/1000.webp 1000w,
                        images/1400.webp 1400w"
                type="image/webp">
        <source srcset="images/400.png 400w, 
                        images/1000.png 1000w,
                        images/1400.png 1400w"
                type="image/png">
        <img
                src="images/400.jpg"
                alt="">
    </picture>
```
Теперь браузер буте отрисовывать оптимальную по ширине картинку в зависимости от размера экрана

### !!Загрузив страницу и изменяя ширину в большую сторону - будут подгружаться более подходящие картинки; при изменении в меньшую сторону - новые изображения подгружаться не будут!!

## Полный привод
Разве может быть лучше? Может!

Зачем нам подбирать картинку в зависимости от размера экрана, если блок, в котором лежит наш `picture` максимум 500 или 800 пикселей?
```html
    <picture>
        <source srcset="images/400.webp 400w,
                        images/1000.webp 1000w,
                        images/1400.webp 1400w"
                sizes="(max-width: 800px) 300px, 
                       (max-width: 1600px) 1000px, 
                       100vw"
                type="image/webp">
        <source srcset="images/400.png 400w,
                        images/1000.png 1000w,
                        images/1400.png 1400w"
                sizes="(max-width: 800px) 300px, 
                       (max-width: 1600px) 1000px, 
                       100vw"
                type="image/png">
        <img
                src="images/400.jpg"
                sizes="(max-width: 800px) 300px, 
                       (max-width: 1600px) 1000px, 
                       100vw"
                alt="">
    </picture>
```
Теперь картинки будут выводится именно те, которые нам нужны


## Стандарт

```html
<picture>
    <source srcset="images/400.webp 400w,
                            images/1000.webp 1000w,
                            images/1400.webp 1400w"
            sizes="(max-width: 800px) 300px,
                           (max-width: 1600px) 1000px,
                           100vw"
            type="image/webp">
    <source srcset="images/400.png 400w,
                            images/1000.png 1000w,
                            images/1400.png 1400w"
            sizes="(max-width: 800px) 300px,
                           (max-width: 1600px) 1000px,
                           100vw"
            type="image/png">
    <img
            src="images/400.jpg"
            sizes="(max-width: 800px) 300px,
                           (max-width: 1600px) 1000px,
                           100vw"
            alt="Обязателен"
            width="400"
            height="114">
</picture>
```
