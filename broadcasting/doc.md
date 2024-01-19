Загальна інформація

- broadcasting працює на веб-сокетах, що у свою чергу створює постійне підключення між клієнтом і сервером. За допомогою цього, сервер може відправляти інформацію клієнтам які підписались на канал. 
- основна концепція broadcasting в Ларавел є проста: на бек-енді створюємо event-клас, реалізуємо метод `broadcastOn()`. В цьому методі вказуємо тип (`private`, `public`, `presence`) та назву каналу, далі клієнти (браузери) підключаються до каналів по назві каналу. Далі, бек-енд відправляє event, а фронт-енд підписуться на канал та слухає повідомлення.
- на прикладі нижче є event `OrderCreatedEvent`, в ньому є метод `broadcastOn()`, де вказано тип та назву каналу. Також, є публічний проперті `$order`, скажімо це модель яка містить інформацію про замовлення. В цьому випадку ця публічна проперті `$order` (чи будь яка інша публічна проперті, якщо б вона існувала) буде десиреалізована та відправлена в канал.
```
use Illuminate\Broadcasting\Channel;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Broadcasting\PrivateChannel;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class OrderCreatedEvent implements ShouldBroadcast
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public Order $order;

    public function broadcastOn(): array
    {
        return [
            new Channel('OrderCreated.' . $this->order->id),
        ];
    }
}
```
- щоб відправити цей event можна використати стандартні методи `OrderCreatedEvent::dispatch()`, `event(new OrderCreatedEvent())` або `broadcast(new OrderCreatedEvent())`
- `ShouldBroadcast` - інтерфейс який потрібно реалізувати, щоб працював broadcasting
- broadcasting працює із queue. Тобто, коли ми робимо `OrderCreatedEvent::dispatch()`, то цей event відправляється в чергу, і далі із черги потрібно щоб воркер опрацював цей event.
- `ShouldBroadcastNow` - інтерфейс, який означає що хочемо одразу виконати event (не відправляти в чергу). 
- за замовчуванням, broadcasting працює із двума офіційними драйверами `Pusher` та `Ably` (вони є комерційні), та ком'юніті пакети `laravel-websockets` та `soketi`
- для локальної розробки Ларавел підтримує `Pusher Channels`, `redis`, та `log`

---

Налаштування для розробки 

- для локальної розробки вибрати, одну із опцій `Pusher Channels`, `Redis` чи `log`
- в `config/app.php` потрібно розкоментувати `App\Providers\BroadcastServiceProvider`
- налаштувати queue та воркери
- якщо використовується `Pusher Channels`, то встановити `composer require pusher/pusher-php-server`. Далі, потрібно буде встановити Pusher креденшили в `.env` файл. Креденшили можна взяти після реєстрації на сайті Pusher.
- на стороні клієнта (фронт-енд), встановити пакет `laravel echo`, та `pusher-js` які необхідні щоб слухати events. Команда - `npm install --save-dev laravel-echo pusher-js`
- щоб встановити `ably`, `laravel-websockets` чи `soketi`, треба читати їх документації по встановленні

---

Канали

Існує 3 види каналів: `Public`, `Private`, `Presence`

`Public`
- `public` - означає що будь хто, може долучитись до нашого каналу та слухати events які відправляються сервером. Тобто, це може бути як авторизований так і не авторизований користувач в системі. Це може бути корисним, коли в нас є наприклад якась статистика на нашому сайті, і ми хочемо щоб цю статистку бачили всі користувачі сайті в real-time. Але якщо, в нас наприклад є сутність `Order`, і ми хочемо відправити інформацію по цьому замовленні до користувача хто є автором замовлення, то публічний канал не підійде для цих цілей, так як інформація по цьому замовленні - це індивідуальна інформація користувача. А через публічний канал її можуть прочитати інші клієнти. Для цього випадку потрібно використати `private` канал.

`Private`
- `private` - означає що тільки авторизований користувач в системі може долучитись до каналу та слухати events. Знову приклад із `Order`, якщо хочемо відправити користувачу інформацію по його замовленні, то `private` канал якраз добре підійде для цього випадку.
- також, для `private` каналу, в файлі `routes/channels.php`, ми вказуємо логіку по авторизації користувача до каналу. Наприклад, нижче ми перевіряємо що користувач який пробує слухати канал, дійсно є автором `Order`. 
- наприкладі нижче, в нас є канал `orders.{id}` - де підставляється id замовлення. І в коллбек функцію прокидується це id замовлення та авторизований користувач в системі, і далі робимо перевірку чи належить цей order до авторизованого користувача в системі. Тут ми можемо використати іншу логіку яка нам потрібна, головне щоб із коллбека поверталось `bool` значення.
- бек-енд
  ```
    use App\Models\Order;
    use App\Models\User;
    
    Broadcast::channel('orders.{orderId}', function (User $user, int $orderId) {
        return $user->id === Order::findOrFail($orderId)->user_id;
    });
  ```
- фронт-енд
  ```
  Echo.private(`orders.${orderId}`)
    .listen('OrderShipmentStatusUpdated', (e) => {
        console.log(e.order);
    });
  ```
  
`Presence`
- `presence` - цей канал так як і приватний канал, використовується тільки для авторизованих користувачів в системі. Відмінність від `private` каналу в тому, що цей канал надає додатковий функціонал.
- найпростіший приклад щоб зрозуміти додатковий функціонал, наступне. Ми маємо спільний чат на сайті в якому є постійнно додаються та видаляються користувачя. Ми хочемо показувати повідомлення всім користувачам коли хтось додається / видаляється із каналу.
- в `routes/channels.php` для `private` каналу ми писали логіку аутентифікації (чи може поточний користувач слухати даний канал), і повертали `true` або `false`. В `presence` це трохи по-іншому працює, замість повернення `true` або `false`, в `presence` каналі нам потрібно повернути масив даних які будуть доступні слухачам на фронт-енді (в прикладі нижче буде повернуто тільки `id` та `name` користувача). Або, якщо користувач не пройшов аутентифікацію, то повертаємо `false` або `null`.
```
  Broadcast::channel('chat.{roomId}', function (User $user, int $roomId) {
      if ($user->canJoinRoom($roomId)) {
          return ['id' => $user->id, 'name' => $user->name];
      }
      
      return false;
});
```
- на фронт-енді потрібно використати метод `join`
```
  Echo.join(`chat.${roomId}`)
```
- і також нам доступні додаткові методи: `here`, `joining`, `leaving`, `error`
```
  Echo.join(`chat.${roomId}`)
      .here((users) => {
          // ...
      })
      .joining((user) => {
          console.log(user.name);
      })
      .leaving((user) => {
          console.log(user.name);
      })
      .error((error) => {
          console.error(error);
      });
```
- `here` - спрацьовує при ініціалізації каналу
- `joining` - коли додається новий користувач в канал
- `leaving` - коли видаляється користувач із каналу
- `error` - коли API авторизації повертає не 200 HTTP код 

--- 

Авторизація

- як згадувалось вище, для `private`, `presence` - каналів, потрібно щоб користувач був авторизований в системі, і далі в `routes/channels.php` ми можемо реалізувати логіку, чи може користувач слухати канал чи ні, приклад нижче.
```
    Broadcast::channel('orders.{orderId}', function (User $user, int $orderId) {
        return $user->id === Order::findOrFail($orderId)->user_id;
    });
```
- так як, це реалізовується в колл-бек функції, то логіка цієї функції може бути велика і якщо в нас є багато каналів, то файл `routes/channels.php` - може сильно збільшитись, та стати складним для читання.
- в Ларавел ми можемо створити `Channel`, і в цей канал перенести цю логіку, приклад нижче:


- `artisan command`
```
php artisan make:channel OrderChannel
```
- файл `routes/channles.php`
```
Broadcast::channel('orders.{order}', OrderChannel::class);
```
- реалізація
```
class OrderChannel
{
    /**
     * Create a new channel instance.
     */
    public function __construct()
    {
        // ...
    }
 
    /**
     * Authenticate the user's access to the channel.
     */
    public function join(User $user, Order $order): array|bool
    {
        return $user->id === $order->user_id;
    }
}
```
- як видно із прикладу вище, то в методі `join()` реалізовуємо логіку по аутентифікації юзера

---

Іще додатково про методи які доступні на бек-енді та фронт-енді

Бек-енд
- `broadcastOn()` - повертаємо масив каналів (`public`, `private`, `presence`) куди транслювати дані.
- `broadcastAs()` - коли ми транслюємо інформацію, то ми створюємо канал (наприклад `orders`), і в цей канал можемо транслювати різні events (`OrderCreated`, `OrderUpdated`, etc). За замовчуванням, назва івента - це назва класу, але за допомогою метода `broadcastAs()`, ми можемо змінити назву івента, наприклад
  - бек-енд
    ```
      public function broadcastAs(): string
      {
          return 'task.created';
      }
    ```
  - фронт-енд
    ```
        Echo.private(`orders.${orderId}`)
            .listen('.task.created', e => {
              console.log(e)
        })
    ```
  - на прикладі вище на бек-енді ми назвали event як `task.created`, на фронт-енді ми повинні слухати event `.task.created`, тобто додати `.` перед назвою event
- `broadcastWith()` - за замовчуванням всі публічні проперті в event класі сериалізуються та будуть відправлені клієнтам. Якщо в нас є наприклад модель User, і ми не хочемо транслювати всі дані моделі User, ми можемо використати цей метод щоб вказати які самі дані хочемо відправляти клієнтам
  ```
      public function broadcastWith(): array
      {
          return ['name' => $this->user->name];
      }
  ```
- `broadcastQueue()` - вказуємо в яку чергу хочемо помістити event
- `broadcastWhen()` - інколи, ми хочемо відправити дані при певних умовах, в цьому методі ми можемо реалізувати логіку чи потрібно відправляти дані, цей метод повинен повертати `true` або `false`
- `via()` - можемо вказати через який драйвер хочемо відправити дані, наприклад `broadcast(new OrderShipmentStatusUpdated($update))->via('pusher');`
- <span style="color:red;">toOthers</span>

Фронт-енд
- `channel` - слухаємо публічний канал
- `listen` - вказуємо який саме event в цьому каналі хочемо слухати
```
Echo.channel(`orders.${this.order.id}`)
    .listen('OrderShipmentStatusUpdated', (e) => {
        console.log(e.order.name);
    });
```
- `private` - якщо хочемо слухати приватний канал, то використовуємо цей метод
```
Echo.private(`orders.${this.order.id}`)
    .listen(/* ... */)
```
- `stopListening` - перестати слухати конкретний івент, але залишитись в каналі
```
Echo.private(`orders.${this.order.id}`)
    .stopListening('OrderShipmentStatusUpdated')
```
- `leaveChannel` - залишити канал

---

Model Broadcasting

- якщо ми по якісь причині не хочемо створювати event, і при цьому хочемо транслювати дані коли над моделю відбуваються якісь дії
- ми можемо робити broadcasting моделі при наступних діях над моделю `created`, `updated`, `deleted`, `trashed`, `restored`
- щоб використати broadcasting на моделі, потрібно в моделі підключити трейт `BroadcastsEvents` та реалізувати метод `broadcastOn()`
```
    public function broadcastOn(string $event): array
    {
        return [$this];
    }
```
- із прикладу вище бачимо, що метод отримує `$event`, в цій перемінній зберігається інформація який саме event був отриманий. Щоб в подальшому ми могли контролювати які івенти хочемо транслювати а які ні, див приклад нижче
```
  public function broadcastOn(string $event): array
  {
      return match ($event) {
          'deleted' => [],
          default => [$this, $this->user],
      };
  }
```
- дані будуть транслювати в приватний канал. На фронт-енді потрібно, слухати івент із префіксом `.`, так сама як було описано вище. 
- на моделі ми так само можемо створити методи `broadcastAs()`, `broadcastWith()`, які описувались вище

---

Тільки фронт-енд

- є деякі методи які можемо використати на фронт-енд без залучення бек-енда. Наприклад, якщо користувач починає друкувати повідомлення, то ми хочемо показати це в чаті для інших користувачів - 'John Doe is typing'. Для такого випадку ми можемо використати методи нижче.
- `whisper` - в цьому методі вказуємо `typing`, та дані які хочемо відправити. Тобто, зараз коли поточний користувач набирає повідомлення, то буде відправлене його ім'я
```
  Echo.private(`chat.${roomId}`)
      .whisper('typing', {
          name: this.user.name
      });
```
- `listenForWhisper` - і цей метод відповідно призначений для прослуховування events.
```
  Echo.private(`chat.${roomId}`)
      .listenForWhisper('typing', (e) => {
          console.log(e.name);
      });
```

---

<span style="color: red;">Notifications</span>

---