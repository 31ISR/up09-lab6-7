В случае работы локально (в колледже) убедитесь, что у вас прокинуты [ переменные прокси и активированы нужные расширения PHP ](https://github.com/31ISR/up09-index?tab=readme-ov-file#%D0%B4%D0%BE%D0%B1%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-proxy-%D0%B4%D0%BB%D1%8F-%D1%82%D0%B5%D1%80%D0%BC%D0%B8%D0%BD%D0%B0%D0%BB%D0%BE%D0%B2)

# Лабораторная работа 6-7

## Дедлайн сдачи работы без пенальти

???

## Как выполнять

- Создать ветку `mod-7` из ветки `mod-6`

- Открыть ветку `mod-7` в workspace

## Форма регистрации и логина

_Для этого можно использовать пакет __breeze__, который позволяет легко имплементировать данный функционал_

>[!WARNING]
>Сделайте бэкап ваших файлов `web.php` и все `css` файлы, так как их перетрет breeze

### Установка пакета breeze

```bash
composer require laravel/breeze --dev
```

### Инициализация регистрации

```bash
php artisan breeze:install
```

1. Выберите `blade` как стэк
2. `no` на даркмод
3. Пропустите шаг про фреймворк для тестов

### Восстановление бэкапа

1. Верните CSS файлы

2. Удалите маршруты `welcome` и `dashboard`

3. Восстановите пути `note` и `todo` из бэкапа файла

```php
<?php
Route::redirect('/', '/note')->name('dashboard');

Route::middleware(['auth', 'verified'])->group(function () {
    Route::resource('note', NoteController::class);
});

Route::middleware('auth')->group(function () {
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');
});
```
- `Route::redirect('/', '/note')->name('dashboard');` — перенаправляет с корня сайта (/) на /note и называет маршрут dashboard.
- `Route::middleware(['auth', 'verified'])->group(...)` — группа маршрутов, доступных только аутентифицированным и подтверждённым пользователям:
    - `Route::resource('note', NoteController::class);` — создаёт все CRUD-маршруты (index, create, store, show, edit, update, destroy) для ресурса note.
- `Route::middleware('auth')->group(...)` — маршруты, доступные только авторизованным пользователям:
    - `GET /profile` — форма редактирования профиля.
    - `PATCH /profile` — обновление профиля.
    - `DELETE /profile` — удаление профиля.

4. Перебейте шаблон `x-layout` на `x-app-layout` во всех шаблонах

> [!WARNING]
> Измените навигацию `navigation.blade.php`
>
> - Замените все ссылки на роут `dashboard` на `note`
> - Добавьте ссылку на экран с задачами

> [!ERROR]
> Для регистрации аккаунта необходимо перейти по пути `/register` (его можно добавить сразу в навигацию для удобства)

### Фильтрация заметок по пользователю

Когда пользователь открывает заметки он должен видеть только те, которые создал он.

Для этого необходимо возвращать пользователю в шаблон, где показываются все заметки, результат данного выражения

```php
Note::query()
            ->where('user_id', request()->user()->id)
            ->orderBy('created_at', 'desc')
            ->paginate();
```

__Не забудьте перебить юзер айди при создании заметки пользователем со значения `1` на айди пользователя - `$request->user()->id;`__

_Проверьте, что все заметки у нового пользователя пропали, а новые заметки отображаются при создании_

### Ограничение просмотра заметок для других юзеров

Если пользователь открывает заметку, которая не принадлежит ему, то он должен быть перенаправлен на `FORBIDDEN` экран. Это делается при помощи вызова функции `abort`

```
if ($note->user_id !== request()->user()->id) {
    abort(403);
}
```

> [!WARNING]
> Добавьте проверку на пути редактирования и удаления заметки

### Редактирование сервисных экранов (403, 404 и тд...)

Создайте папку `errors` внутри папки с представлениями и создайте представление `403.blade.php`

```php
<x-app-layout>
    <h1>Forbidden</h1>
</x-app-layout>
```

> [!WARNING]
> Добавьте экраны ошибок для 400, 401, 404, 405, 408 и добавьте стилей

> [!WARNING]
> Проверьте весь функционал проекта

# Проект закончен
