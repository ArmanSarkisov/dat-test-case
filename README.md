Нужно сделать приложение которое будет собираться в два файла plugin.js plugin.css и будет встраиваться в любое приложение через window. Нужно создать проект в github и передать нам ссылку по окончанию задания

Пример вызова приложения:

```js
document.addEventListener("DOMContentLoaded", () => {
  window.MY_APP &&
    window.MY_APP.init({
      selector: "#root"
    });
});
```

Приложение должно быть написано на стеке

**React, Typescript, Effector, Vite**

и иметь опции

```js
document.addEventListener("DOMContentLoaded", () => {
  window.MY_APP &&
    window.MY_APP.init({
      selector: "#root",
      options: {
        initializedOptions: [A, B, C, D], // массив активных позиций
        onPositionChange(positions) {
          // вызывается на изменнеие позиций и возвращает массив активных позиций
          console.log(postions);
        },
        onComplete(positions) {}, // вызывается при подтверждении событий
        onInit() {} // Вызывается на инициализацию плагина
      }
    });
});
```

Это можно реализовать с помощью обертки над render функцией:

```js
if (import.meta.env.MODE  ===  'development') {
  const  renderElement  =  document.getElementById('root');
  createRoot(renderElement  as  HTMLElement).render(
    <App
      options={{
         ...someOPtionsOnlyForDev
      }}
    />
  );
} else {
	window.DAMAGE_SELECTOR_API  = {
		init: (options: PluginOptions) => {
			const { selector } = options;
			if (selector) {
				const  renderElement = document.querySelector(selector);
				if (renderElement) {
					createRoot(renderElement as HTMLElement).render(
						<ErrorBoundary>
							<App  options={options}  />
						</ErrorBoundary>
					);
				}
			}
		}
	};
}
```

Приложение должно выглядеть вот так:
![enter image description here](https://i.ibb.co/gPjQxxY/Screenshot-2023-07-10-141447.png)

- Изображение автомобиля и точки указанные на ней это кнопки которые указывают на повреждения автомобиля.

- Каждый кружочек имеет свою букву и при нажатии на эту кнопку мы вызываем callback onPositionChange и передаем массив с активными точками в callback.
- Если точка активна то она становится оранжевой
- При нажатии на кружочек срабатывает onComplete callback в который передается массив позиций

Схема позиций:
![enter image description here](https://i.ibb.co/j41F55z/Screenshot-2023-07-10-142359.png)

Effector:
При инициализации плагина через effector мы должны сделать запрос на мини апи и получить текущий список позиций:
GET: https://myfailemtions.npkn.net/b944ff/

А при нажатии на кружочек нужно отправить запрос на апи с массивом активных позиций:

POST: https://myfailemtions.npkn.net/b944ff/
`["A1", "B2"]`

Все это должно быть реализованно через effector
