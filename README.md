# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/scss/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```

## Базовый код

1. Класс EventEmitter
Реализует паттерн «Наблюдатель» и позволяет подписываться на события и уведомлять подписчиков о наступлении события.

Класс имеет методы:

```
on(event: string, listener: Function) — подписка на событие.
off(event: string, listener: Function) — отписка от события.
emit(event: string, ...args: any[]) — уведомление подписчиков о наступлении события.
```

2. Класс Component<T>:
Базовый класс для всех компонентов представления.

Конструктор:

```
constructor(protected readonly container: HTMLElement) — принимает контейнер для рендеринга компонента.
```

Методы:

```
toggleClass(element: HTMLElement, className: string, force?: boolean) — переключение класса.
setText(element: HTMLElement, value: string) — установка текстового содержимого.
setDisabled(element: HTMLElement, state: boolean) — изменение состояния блокировки элемента.
setHidden(element: HTMLElement) — скрытие элемента.
setVisible(element: HTMLElement) — отображение элемента.
setImage(el: HTMLImageElement, src: string, alt?: string) — установка изображения.
render(data?: Partial<T>): HTMLElement — рендеринг компонента.
```

## Компоненты модели данных

1. Класс ProductStore
Управляет массивом товаров и предоставляет методы для работы с этим массивом.

Конструктор:

```
constructor(private products: IProduct[]) — принимает массив товаров.
```

Класс имеет такие методы:

```
addProduct(product: IProduct) — добавляет товар в массив товаров.
removeProduct(productId: string) — удаляет товар из массива по его идентификатору.
getProductById(productId: string): IProduct | undefined — возвращает товар по его идентификатору.
getAllProducts(): IProduct[] — возвращает весь массив товаров.
updateProduct(productId: string, updatedData: Partial<IProduct>) — обновляет данные товара по его идентификатору.
```

2. Класс Cart
Управляет товарами в корзине.

Конструктор:

```
constructor(private items: { product: IProduct, quantity: number }[]) — принимает массив объектов, представляющих товары в корзине и их количество.
```

Поля класса:

```
items: { product: IProduct, quantity: number }[] — массив объектов, представляющих товары в корзине и их количество.
totalItems: number — общее количество товаров в корзине.
totalPrice: number — общая стоимость товаров в корзине.
```

Класс имеет такие методы:

```
removeProduct(productId: string) — удаляет товар из корзины по его идентификатору.
getItems() — возвращает список товаров в корзине.
getTotalItems() — возвращает общее количество товаров в корзине.
getTotalPrice() — возвращает общую стоимость товаров в корзине.
```

3. Класс Checkout
Управляет процессом оформления заказа.

Конструктор:

```
constructor(private cart: Cart) — принимает объект корзины.
```

Поля класса:

```
address: string — адрес доставки.
email: string — электронная почта покупателя.
phone: string — телефон покупателя.
paymentMethod: string — способ оплаты.
items: { product: IProduct, quantity: number }[] — массив товаров в корзине, которые будут оформлены в заказе.
```

Класс имеет такие методы:

```
start() — подготавливает данные для оформления заказа. Метод выполняет следующие действия:

- Проверяет, что в корзине есть товары. Если корзина пуста, выбрасывает ошибку или возвращает сообщение о пустой корзине.
- Инициализирует пустые поля адреса, электронной почты, телефона и способа оплаты.
- Копирует текущие товары из корзины в массив items для оформления заказа.
- Устанавливает начальное состояние процесса оформления заказа, например, шаг процесса (выбор способа оплаты, ввод контактных данных и т.д.).
- enterAddress(address: string) — вводит адрес доставки.

enterContactDetails(email: string, phone: string) — вводит контактные данные.

selectPaymentMethod(paymentMethod: string) — выбирает способ оплаты.

completeOrder() — завершает заказ, если все данные введены корректно. Метод выполняет следующие действия:

- Проверяет корректность введенных данных (адреса, контактных данных, способа оплаты).
- Если данные некорректны, возвращает сообщение об ошибке и останавливает процесс завершения заказа.
- Если данные корректны, создает объект заказа с данными о товарах, адресе доставки, контактной информации и способе оплаты.
- Отправляет данные заказа на сервер для обработки.
- Ожидает подтверждения успешной обработки заказа от сервера.
- Если заказ успешно обработан, очищает корзину и обновляет состояние приложения.
- Уведомляет пользователя о успешном завершении заказа (например, отображает сообщение или перенаправляет на страницу с подтверждением заказа).
```

4. Класс ApiService:
Обеспечивает взаимодействие с сервером.

Конструктор:

```
constructor(baseUrl: string, options: RequestInit = {}) — принимает базовый URL и опции для запросов.
```

Методы:

```
async get(uri: string) — выполняет GET запрос на указанный URI.
async post(uri: string, data: object) — выполняет POST запрос на указанный URI с передачей данных.
```

## Компоненты представления

1. Компонент ProductList
Отображает каталог товаров.

Конструктор:

```
constructor(container: HTMLElement, protected events: EventEmitter) — принимает контейнер для рендеринга и объект для управления событиями.
```

Функции компонента:

- Отображение списка товаров.

Методы:

```
set store(items: IProduct[]) — устанавливает массив товаров и рендерит их.
render(): HTMLElement — рендеринг компонента.
```

2. Класс ProductDetails
Отвечает за отображение информации о товаре. Генерирует разметку для представления товара.

Конструктор:

```
constructor(private product: IProduct) — принимает объект товара.
```

Функции класса:

- Отображение детальной информации о товаре.
- Генерация HTML разметки для отображения информации о товаре.

Методы:

```
render(): HTMLElement — генерирует HTML разметку для отображения информации о товаре.
```

3. Компонент ProductCard
Отображает информацию о товаре в виде карточки.

Конструктор:

```
constructor(protected blockName: string, container: HTMLElement, actions?: ICardActions) — принимает имя блока, контейнер и объект с действиями.
```

Функции компонента:

- Отображение информации о товаре.
- Включение ProductDetails для генерации разметки товара.

Методы:

```
render(): HTMLElement — рендеринг компонента.
```

4. Компонент ProductModal
Отображает модальное окно с детальной информацией о товаре.

Конструктор:

```
constructor(container: HTMLElement, protected events: EventEmitter) — принимает контейнер для рендеринга и объект для управления событиями.
```

Функции компонента:

- Открытие модального окна.
- Включение ProductDetails для отображения информации о товаре.
- Закрытие модального окна.

Методы:

```
render(): HTMLElement — рендеринг компонента.
```

5. Компонент CartComponent
Отображает товары в корзине.

Конструктор:

```
constructor(container: HTMLElement, protected events: EventEmitter) — принимает контейнер для рендеринга и объект для управления событиями.
```

Функции компонента:

- Отображение списка товаров в корзине.

Методы:

```
set cartItems(items: IProduct[]) — устанавливает массив товаров в корзине и рендерит их.
render(): HTMLElement — рендеринг компонента.
```

6. Компонент CheckoutComponent
Отвечает за интерфейс процесса оформления заказа.

Конструктор:

```
constructor(container: HTMLElement, protected events: EventEmitter) — принимает контейнер для рендеринга и объект для управления событиями.
```

Функции компонента:

- Отображение формы для ввода адреса доставки.
- Отображение формы для ввода контактных данных.
- Отправка данных формы в модель для валидации и обработки.

Методы:

```
render(): HTMLElement — рендеринг компонента.
```

##Взаимодействие частей
1. Взаимодействие между моделями данных и компонентами представления

Модели данных (ProductStore, Cart, Checkout) взаимодействуют с компонентами представления (ProductList, ProductCard, ProductModal, CartComponent, CheckoutComponent) через события. Это позволяет компонентам представления обновлять интерфейс в ответ на изменения данных, обеспечивая при этом чистую архитектуру с разделением обязанностей.

2. Взаимодействие с сервером через ApiService

Модели данных используют ApiService для взаимодействия с сервером. ApiService предоставляет методы для выполнения HTTP-запросов, такие как get и post, и обработки ответов от сервера.

## Ключевые типы данных

```
//Перечисление всех возможных категорий товаров
type CategoryType =
  | 'другое'
  | 'софт-скил'
  | 'дополнительное'
  | 'кнопка'
  | 'хард-скил';

//Тип, описывающий ошибки валидации форм.
type FormErrors = Partial<Record<keyof IOrderForm, string>>;

//Интерфейс, описывающий карточку товара в магазине
interface IProduct {
  id: string;
  description: string;
  image: string;
  title: string;
  category: CategoryType;
  price: number | null;
  selected: boolean;
}

// Интерфейс, описывающий внутреннее состояние приложения
interface IAppState {
  basket: IProduct[];
  store: IProduct[];
  order: IOrder;
  formErrors: FormErrors;
  addToBasket(value: IProduct): void;
  deleteFromBasket(id: string): void;
  clearBasket(): void;
  getBasketAmount(): number;
  getTotalBasketPrice(): number;
  setItems(): void;
  setOrderField(field: keyof IOrderForm, value: string): void;
  validateContacts(): boolean;
  validateOrder(): boolean;
  refreshOrder(): boolean;
  setStore(items: IProduct[]): void;
  resetSelected(): void;
}

//Интерфейс, описывающий поля заказа товара
interface IOrder {
  items: string[];
  payment: string;
  total: number;
  address: string;
  email: string;
  phone: string;
}

// Интерфейс, описывающий карточку товара
interface ICard {
  id: string;
  title: string;
  category: string;
  description: string;
  image: string;
  price: number | null;
  selected: boolean;
}

//Интерфейс, описывающий страницу
interface IPage {
  counter: number;
  store: HTMLElement[];
  locked: boolean;
}

//Интерфейс, описывающий корзину товаров
interface IBasket {
  list: HTMLElement[];
  price: number;
}

//Интерфейс, описывающий контактные данные
interface IContacts {
  phone: string;
  email: string;
}

```
