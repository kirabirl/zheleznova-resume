# Ira Portfolio

Одностраничное портфолио UX/UI-дизайнера на Vue 3 + JavaScript + Vite.

## Локальный запуск

```bash
npm install
npm run dev
```

## Что менять

Основной контент лежит в `src/App.vue`:

- `email`, `telegram`, `hhUrl` — контакты и внешние ссылки.
- `experience` — опыт работы.
- `skillGroups` — блок навыков.
- `projects` — карточки галереи и содержимое попапа проекта.

У каждого проекта есть поля:

- `title` — название карточки и страницы проекта.
- `shortTitle` — подпись внутри временного превью.
- `accent` — цвет временного превью.
- `figmaUrl` — публичная ссылка на макет Figma.

В текущем варианте проекта для встраивания интерактивного макета используется поле `figmaUrl`.
Вставь туда публичную ссылку Figma с доступом `Anyone with the link can view`. Сайт автоматически
покажет ее в iframe внутри кейса.

## Деплой: GitHub repo → Cloudflare Pages

### 1. Подготовить GitHub-репозиторий

Сейчас в папке проекта есть пустой каталог `.git`, но это не рабочий Git-репозиторий. Если `git init` ругнется, удали пустую папку:

```bash
rmdir .git
```

Потом инициализируй репозиторий:

```bash
git init
git add .
git commit -m "Initial portfolio site"
git branch -M main
git remote add origin https://github.com/USER/ira-portfolio.git
git push -u origin main
```

`USER` замени на свой GitHub username. Репозиторий на GitHub нужно создать заранее.

### 2. Подключить Cloudflare Pages

1. Зайди в Cloudflare Dashboard.
2. Открой `Workers & Pages`.
3. Нажми `Create application`.
4. Выбери `Pages`.
5. Нажми `Connect to Git`.
6. Авторизуй GitHub и выбери репозиторий `ira-portfolio`.

### 3. Настройки сборки

В Cloudflare Pages укажи:

- Framework preset: `Vue`
- Build command: `npm run build`
- Build output directory: `dist`
- Root directory: оставить пустым, если проект лежит в корне репозитория

После деплоя Cloudflare выдаст бесплатный URL вида:

```text
https://ira-portfolio.pages.dev
```

### 4. Как обновлять сайт

После любых правок:

```bash
git add .
git commit -m "Update portfolio"
git push
```

Cloudflare Pages сам пересоберет сайт после push в `main`.
