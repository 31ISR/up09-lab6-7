# Задание еще не закончено

---

## Добавление proxy для composer

### Добавление для шелла

```powershell
set HTTP_PROXY=http://10.0.55.52:3128
set HTTPS_PROXY=http://10.0.55.52:3128
```

### Добавление для композера

```powershell
composer config -g -- http-proxy http://10.0.55.52:3128
composer config -g -- https-proxy http://10.0.55.52:3128
```

# Лабораторная работа 6-7

## Дедлайн сдачи работы без пенальти

???

## Как выполнять

- Создать ветку `mod-7` из ветки `mod-6`

- Открыть ветку `mod-7` в workspace

## Форма регистрации и логина

_Для этого можно использовать пакет _breeze_, который позволяет легко имплементировать данный функционал_

>[!WARNING]
>Сделайте бэкап ваших файлов `web.php` и все `css` файлы, так как их перетрет breeze

### Установка пакета breeze

```bash
composer require laravel/breeze --dev
```

Если не получилось, то можете вручную установить зависимости из папки `vendor` и добавить следующий пакет в `composer.json`

```json
    "require-dev": {
        ...
        "laravel/breeze": "^2.3"
        ...
    }
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
> Добавьте навигацию между заметками и задачами
