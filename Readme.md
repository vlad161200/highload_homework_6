# Аналіз архітектури Instagram для функцій лайків і коментарів

## Вузькі місця (Bottlenecks)

### Load Balancer
- Хоча балансувальник навантаження розподіляє трафік між веб-серверами, він може стати вузьким місцем, якщо кількість запитів значно зросте, особливо якщо балансувальник сам по собі не масштабується належним чином.

### Web Server
- Веб-сервери можуть стати вузьким місцем, якщо їх кількість або обчислювальні ресурси (CPU, пам'ять) недостатні для обробки великої кількості запитів від клієнтів.

### Queue
- Якщо черга переповнюється через велику кількість операцій запису, це може уповільнити виконання завдань, особливо якщо система обробки черг не налаштована на автоматичне масштабування.

### Write API
- Сервіси Write API і Write API Async можуть стати вузькими місцями, якщо кількість операцій запису (наприклад, лайків або коментарів) перевищить їх здатність обробляти ці запити.

### Memory Cache
- Кешування даних допомагає знизити навантаження на бази даних, але якщо кеш не налаштований або не масштабується належним чином, це може стати вузьким місцем, особливо за високих навантажень на читання даних.

### SQL Write Master-Slave
- База даних, налаштована в режимі майстра/репліки, може стати вузьким місцем у разі великого обсягу операцій запису, особливо якщо продуктивність майстер-сервера недостатня.

### Read API
- Подібно до Write API, Read API може стати вузьким місцем у разі високих запитів на читання даних, якщо кількість серверів або їхні ресурси обмежені.

## Точки відмови (SPOFs)

### Load Balancer
- Балансувальник навантаження є критичною точкою відмови. Якщо він вийде з ладу, усі клієнти втратять доступ до веб-серверів.

### Queue
- Якщо черга вийде з ладу, це призведе до зупинки всіх асинхронних операцій, таких як обробка лайків, коментарів і повідомлень.

### SQL Write Master-Slave
- Якщо майстер-сервер бази даних вийде з ладу, операції запису будуть неможливі, що призведе до збою в роботі функціональності лайків і коментарів.

### Notification Service
- Якщо сервіс повідомлень вийде з ладу, користувачі не отримуватимуть сповіщення про нові лайки та коментарі.

### Memory Cache
- Якщо кеш вийде з ладу, система буде змушена запитувати дані безпосередньо з бази даних, що може значно збільшити затримки і навантаження на БД.
