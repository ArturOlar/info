Laravel Passport

- це повна реалізація протоколу OAuth2 зі сторони сервера
- важливо розуміти, що `passport` потрібно використовувати якщо ви точно знаєте що вам потрібна реалізація протоколу OAuth2
- в іншому випадку, якщо вам потрібно аутентифікація для: SPA, mobile app, видавання API-токенів, то потрібно використовувати `sanctum`, який не підтримує OAuth2, але є простішим у роботі

---

Налаштування & конфігурації

- `passport` повинен створити необхідні таблиці для зберігання інформації по токенах, тому після встановлення потрібно запустити міграції та `php artisan passport:install` 
- також, потрібно в моделі `User` додати трейт `Laravel\Passport\HasApiTokens`, та `config/auth.php` встановити драйвер для `api` як  `passport`
- за замовчуванням токени `access_token`, `refresh_token` видаються на 1 рік. Цей термін можна змінивши, приклад нижче:
```
public function boot(): void
{
    Passport::tokensExpireIn(now()->addDays(15));
    Passport::refreshTokensExpireIn(now()->addDays(30));
    Passport::personalAccessTokensExpireIn(now()->addMonths(6));
}
```
- за замовчуванням, `passport` має всі необхідні роутери для: створення/видалення клієнта чи токена, обміну `code` на реальний `access_token` і тд. Та при цьому ми можемо ці роутери змінити на наші.
- клієнт означає те саме що і в звичайній термінології OAuth2 - тобто, аплікейшн який хоче отримувати інформацію про Resource Owner

---

Authorization Code Grant With PKCE (Proof Key for Code Exchange)

- один із варіантів як можна використовувати протокол OAuth, вцілому він застосовується для: SPA, моб додатків, тоді коли `client_secret` не може бути безпечно збережений в системі. Суть полягає в тому, щоб відправляти `code_verifier` та `code_challenge` (`code_verifier` - це набір рандомних символів/букв/цифр, `code_challenge` - це захешований `code_verifier`, який був закодований в bas64 формат). Тобто, клієнт коли генерує redirect-лінк він включає `code_challenge` і вказує яким алгоритмом хешування ці дані були хешовані (для прикладу `SHA256`). В наступному реквесті коли клієнт відправляє реквест на отримання `access_token` відправляється `code_verifier`. Далі, сервер пробує `code_verifier` захешувати і перевести в формат base64, і порівняє чи це таке саме значення яке було в `code_challenge`. Вцілому, це досить звичайний варіант, щоб зрозуміти чи реквест на отримання `access_token` і початковий реквест де був redirect-лінк були відправлені від одного сервера. 

---

Password Grant Tokens (DEPRECATED)

- один із варіантів використання OAuth, який являється застарілим та не рекомендується для використання. Основний недолік цього метода полягає в тому, що щоб отримати `access_token`, потрібно щоб клієнт відправляв свої дані: email, пароль. І очевидно що ці дані можуть бути перехоплені, чи клієнт збереже собі ці дані, і тд. Тобто, цей спосіб не являється безпечним

---

Implicit Grant Tokens (DEPRECATED)

- один із варіантів використання OAuth, який являється застарілим та не рекомендується для використання. Він дуже схожний до класичного метода OAuth із використанням `code`, який клієнт обмінює на `access_token`. Тільки в цьому випадку `access_token` одразу відправляється до resource owner, і далі до клієнта. Очевидно, що цей `access_token` може бути перехоплений та використовуватись зловмисниками.

---

Personal Access Tokens

- наскільки зрозумів, то цей підхід не являється частиною протоколу OAuth2
- вцілому ідея наступна. Користувачі (resource owner) вашого сайту, можуть створювати самостійно `access_token`, без потреби проходит 'code redirect flow'
- варто зазначити, якщо ваш аплікейшн використовує Passport щоб видавати Personal Access Tokens, то тоді краще використовувати Sanctum

---

Token Scopes

- `scopes` - дозволяє клієнтам API запитувати доступ до певних ресурсів та дій над цими ресурсами. Наприклад: в нас на сайті є posts, і в нас можуть бути scopes: `view` (перегляд посту/постів), `manage` (створення/видалення/редагування посту чи постів). Далі, клієнти можуть запитувати лише `view` чи лише `manage` чи можливо обидва `view manage`. Тобто, якщо клієнт запросив тільки `view` scope, то він не буде мати дозволу для `manage` scope. Ці scopes, ми налаштовуємо в нашій системі як нам потрібно.
- щоб створити scopes, потрібно в `AuthServiceProvider` додати наступний код 
```
public function boot(): void
{
    Passport::tokensCan([
        'post-view' => 'Access to view post(s)',
        'post-manage' => 'Access to create, delete, update post(s)',
    ]);
}
```
- також, можна зробити default scopes, тобто якщо клієнт не запитує ніякі scopes, то будуть застосовуватись за замовчуванням:
```
Passport::setDefaultScope([
    'post-view',
    'post-manage',
]);
```
- щоб перевірка для scopes почала працювати, потрібно зробити наступне, в `app/Http/Kernel.php` в масиві `$middlewareAliases` додати наступні мідлвари 
```
'scopes' => \Laravel\Passport\Http\Middleware\CheckScopes::class,
'scope' => \Laravel\Passport\Http\Middleware\CheckForAnyScope::class,
```
- перевірка scopes працює, так як і звичайні мідлвар, приклад
```
Route::get('/posts', function () {
    ...
})->middleware(['auth:api', 'scopes:post-view,post-manage']);
```