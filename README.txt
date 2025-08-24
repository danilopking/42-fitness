# 42 Fitness — Установка на телефон

Дата сборки: 2025-08-24 14:37 

## Вариант А. Самый быстрый (без установки как PWA)
1. Скопируйте файл `index.html` на телефон и откройте его в Chrome.
2. Приложение работает локально, прогресс сохраняется в браузере.
> Это удобно для теста, но без красивой установки.

## Вариант B. Установка как PWA (красивый ярлык, офлайн-кэш)
**Способ 1 (быстро на ПК):**
1. На компьютере установите Node.js (если нет).
2. В папке с проектом запустите локальный сервер, например:
   ```bash
   npx http-server -p 8080
   ```
3. Откройте на телефоне в той же сети `http://IP_вашего_ПК:8080/`
4. В Chrome → ⋮ → «Установить приложение» / «Добавить на главный экран».
5. После установки будет иконка «42 Fitness», офлайн-кэш (sw.js), манифест.

**Способ 2 (GitHub Pages — бесплатно и навсегда):**
1. Создайте репозиторий `42-fitness`.
2. Загрузите файлы из этой папки в корень репозитория.
3. В настройках GitHub включите Pages (Deploy from branch, main, /root).
4. Откройте публичный URL на телефоне → «Установить приложение».

## Вариант C. Полноценный APK (Capacitor + Android Studio, локальные уведомления)
0. Требуется: Node.js, Android Studio, JDK 17.
1. В терминале создайте оболочку проекта:
   ```bash
   mkdir 42-capacitor && cd 42-capacitor
   npm init -y
   npm i @capacitor/core @capacitor/cli
   npx cap init "42 Fitness" "com.example.fortytwo"
   ```
2. Скопируйте PWA-файлы (`index.html`, `manifest.webmanifest`, `sw.js`, папку `icons/`) в папку `42-capacitor/www/`
   ```bash
   mkdir -p www
   cp /path/to/42-fitness-pwa/* www/ -r
   ```
3. Добавьте Android-платформу:
   ```bash
   npm i @capacitor/android
   npx cap add android
   npx cap copy
   ```
4. (Опционально) Локальные уведомления:
   ```bash
   npm i @capacitor/local-notifications
   npx cap sync
   ```
   В `android/app/src/main/AndroidManifest.xml` Capacitor добавит нужные пермишены. В JS можно вызвать:
   ```js
   import { LocalNotifications } from '@capacitor/local-notifications';
   await LocalNotifications.requestPermissions();
   await LocalNotifications.schedule({
     notifications: [{ id: Date.now(), title: '42 Fitness', body: 'Время тренировки', schedule: { at: new Date(Date.now()+60*1000) } }]
   });
   ```

5. Откройте проект в Android Studio и соберите APK:
   ```bash
   npx cap open android
   ```
   В Android Studio: Build → Build Bundle(s)/APK(s) → Build APK(s).
   Установите APK на телефон (перешлите файл или через ADB).

## Импорт/Экспорт данных
- В шапке приложения есть кнопки «Экспорт» / «Импорт» (JSON).

## Где что лежит
- `index.html` — само приложение (минималистичный UI, 5 экранов).
- `manifest.webmanifest` — PWA-манифест + иконки.
- `sw.js` — сервис-воркер для офлайн-кэша.
- `icons/` — иконки (192–512px).
