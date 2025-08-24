
# 42 Fitness — PWA и APK

Этот пакет содержит полностью готовую к установке **PWA**-версию (как «приложение» через браузер) и инструкцию по сборке **APK** без затрат.

## Вариант A — Быстрая установка как PWA (без аккаунтов)

### 1) Через локальный сервер на телефоне
1. Скопируйте папку на телефон (в любую директорию).
2. Установите приложение **Termux** (с F-Droid) и запустите локальный сервер:
   ```bash
   pkg update
   pkg install python -y
   cd /sdcard/<путь-к-папке-42-fitness-pwa>
   python -m http.server 8080
   ```
3. Откройте в Chrome на телефоне адрес `http://127.0.0.1:8080`.
4. Меню ⋮ → **«Установить приложение»** (или «Добавить на главный экран»).
5. Приложение появится на рабочем столе как **42 Fitness** и будет работать **офлайн** (благодаря Service Worker).

### 2) Через GitHub Pages (если не против создать аккаунт)
1. Создайте репозиторий `42-fitness` и загрузите сюда **все файлы** из этой папки.
2. В настройках репозитория включите **Pages** (ветка `main`, папка `/root`).
3. Откройте выданный URL в Chrome → установите PWA (⋮ → «Установить приложение»).

---

## Вариант B — Нативный APK (офлайн, без серверов)

Требуется ПК (Windows/Mac/Linux), **Node.js** и **Android Studio**.

### 0) Подготовка
- Установите Node.js (LTS) и Android Studio.
- В Android Studio установите **Android SDK** и платформу **Android 12+**.

### 1) Создайте папку проекта и скопируйте PWA-файлы
```
mkdir 42-fitness-app && cd 42-fitness-app
mkdir www
# Скопируйте СОДЕРЖИМОЕ папки 42-fitness-pwa в ./www
```

### 2) Инициализация Capacitor
```
npm init -y
npm i @capacitor/core @capacitor/cli @capacitor/android
npx cap init "42 Fitness" com.dl.fortytwofitness --web-dir=www
npx cap add android
```

### 3) Открыть и собрать в Android Studio
```
npx cap open android
```
- В Android Studio: **Build > Build Bundle(s)/APK(s) > Build APK(s)**  
- Готовый APK будет в: `android/app/build/outputs/apk/debug/app-debug.apk`

Установка на телефон:
- Включите «Установка из неизвестных источников».  
- Перенесите APK на телефон и откройте, либо:
```
adb install -r android/app/build/outputs/apk/debug/app-debug.apk
```

Где будут лежать файлы? Внутри APK они упакованы в **assets**, WebView загружает их локально (всё работает офлайн).

### Пуш-напоминания (локальные) — позже
После первой сборки можно добавить локальные уведомления (например, через плагин `@capacitor/local-notifications`) и задать расписание.

---

## Данные и приватность
Все данные (план/журнал) хранятся **локально** (localStorage/WebView storage).  
Вы можете экспортировать/импортировать JSON через кнопки «Экспорт/Импорт».

Удачи и быстрого прогресса! 💪
