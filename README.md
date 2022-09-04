# Диаграмма


Читается диаграмма снизу-вверх

В самом внизу описаны состояния Блокировки мест и Выбора мест:


## SelectPlaceState Abstract

Абстрактный класс выбора мест. Имеет виртуальный метод select и несколько базовых вспомогательных методов.
От него наследуются классы SeatSelectPlaceState и CompartmentSelectPlaceState

### CompartmentSelectPlaceState

Класс реализует логику выбора мест в купе

### SeatSelectPlaceState

Класс реализует логику выбора мест в сидячем вагоне

## DisableState Abstract

Абстрактный класс блокировки мест. имеет виртуальный метод disable. И несколько вспомогательных методов.
От него наследуются классы: 
- AllCompartmentDisableState
- AllowMultiBookingCompartmentDisableState
- CompartmentDisableState
- AllowMultiBookingDisableState
- SeatDisableState
- StaticDisableState

### StaticDisableState

Состояние блокирования мест для статической схемы, блокирует все не выранные места.

### SeatDisableState 

Состояние блокирования мест для сидячих вагонов. Блокирует все, места, которые не рядом.
Постепенно их разблокировывает, пока пользователь не выберет 4 места. И так в обратной порядке, при снятии блокировки.

### AllowMultiBookingDisableState 

Состояние блокировки для мест в вагонах в режиме Мультибукинг. То есть, выбор мест без какого-либо порядка. Потом, при отправке данных для каждого места, будет создана отдельая услуга/заказ. Блокировка мест происходит после выбора всех мест.

### CompartmentDisableState 

Состояние блокировки мест в купе-вагонах. Тут происходит блокировка мест по гендеру. Если мы выбрали какое-то место в купе, то блокируем все остальные.

### AllowMultiBookingCompartmentDisableState

Состояние блокировки для мест в вагонах в режиме Мультибукинг. То есть, выбор мест без какого-либо порядка. Блокировка происходит только по гендеру и если все места выбраны.

### AllCompartmentDisableState

Состояние блокировки мест в купе-вагонах и купе-переговорочных в режимк "выбора всего купе". Блокирует не полные купе. Блокирует купе с другими гендерами. Блокирует другие купе, если уже есть выбранное купе.

## SchemeModel

Класс, который содержит состояние схемы:

- Все места
- Выбранные места
- Все купе
- Выбраные купе
- Изначальный гендер (Если в заказе только женщины, то изначальный гендер "Женский")
- Текущий гендер (То, что выбрал пользователь в выпадашке выбора гендера)
- В каком режиме работет схема
- Доступна ли возможность "Мультибукинга"
- Класс состояния выбора мест
- Класс состояния блокировки мест

### Связь с состояниями

Вышеописанные состояния связаны со SchemeModel по типу связи "Ассоциация", через методы setSelectPlaceState и setDisableState.
Это означает, что мы можем заменить логику выбора мест просто установив другое состояние, без потери уже накомпленного внутреннего состояния в SchemeModel

### Api 

Схема предоставляет методы по доступу к данным внутри схемы (Поиск, фильтрация, просто получение), изменению внутреннего состояния, выбора/удаления мест и купе, установка или сброс гендера и тд. 

При выборе места. Метод **`selectPlace`**, схема вызывает методы **`select`** у состояния выбора, а затем вызывает метод **`disable`**


