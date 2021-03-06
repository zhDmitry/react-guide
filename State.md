# Low and High coupling in state management lib

## Определение

В стейте менеджменте существует несколько сущностей(логически разных елементов)

1.  cигнал который описывает какое-то действие
2.  бизнес логика которая выполняет специфические для приложения действия
3.  мутация и трансформация данных
4.  вычесляемые данные
5.  подписка на те данные, которые изменились

## High coupling

Идея сильного связыванния компонентов состоит в том что мы связываем сущности стейт менеджмента наиболее тесно и тем самым уберием лишние абстракции и выстраиваем все в максимально короткую цепочку.
Если взять в пример mobx, то у нас из всех сущностей стейт менеджмента остается только екшин(1) и вычесляемые данные. Трансформация, бизнес логика, мутация данных делегируется в екшин и структурирование кода остается на плечах разработчика.

Плюсы:

- нету бойлерплейта
- в малых обьемах быстро рефакторится
- легкий старт и простота понимания на начальных этапах разработки

Минусы

- при росте зависимостей и связей между екшинами это все может превратится в кашу
- сложнее дебаг
- что бы сделать приложение менее связным надо использывать дополнительные механизмы типа di
- нету строгости, так что можно легко написать плохой код в неправильном месте

Такой подоход использует большая часть библиотек на линзах и прокси mobx, focal, react-easy-state, тд

## Low coupling

Слабое связывание это отделение вью от стора и бизнес логики с помощью ивентов(сигналов,екшинов).
Суть этого подхода что вью не имеет прямых связей с стором или бизнесл логикой, а бизнес логика не знает ничего со стором, вся коммуникация происходит через механизм ивентов и общую очередь ивентов.

// некоторые из звеньев могут быть прямыми,а не через ивенты
view -> event -> logic -> event -> transformer-> event-> store -> computed -> view

В таком случае достигается минимальная связанность компонентов приложения и тем самым легче рефакторить большие приложения, а очередь ивентов позволяет ослеживать произведенные изменения над данными

Плюсы:

- хорошо показывает себя на больших приложениях
- легче рефакторить запутанную логику
- легко дебажить запутанную логику
- потоки ивентов хорошо преобразовываются в стримы и тем самым можно использывать их для обработки сложных флоу

Минусы:

- абстракции усложняют код
- для простых действий может понадобится много бойлерплейта
- отслеживание сложной цепочки ивентов занимает много времени
- нужны дополнительные абстракции для работы со сложными воркфлоу (rx,saga)

к этому подходу относятся redux, cerebral, elm arhitecture, flux
