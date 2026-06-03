# Ira Portfolio

Одностраничное портфолио UX/UI-дизайнера на Vue 3 + JavaScript + Vite.

## Локальный запуск

```bash
npm install
npm run dev
```

## Что менять

Основной контент лежит в `src/App.vue`:

- `links` — контакты и внешние ссылки.
- `skillGroups` — блок навыков.
- `projects` — карточки галереи и содержимое попапа проекта.

Сейчас обложки проектов сделаны CSS-заглушками. Когда будут реальные изображения:

1. Положи файлы в `src/assets/`.
2. Импортируй их в `src/App.vue`.
3. Импортируй файл в `src/App.vue`, например `import faviasCover from './assets/favias.jpg'`.
4. В нужном проекте замени `imageClass: 'cover-favias'` на `image: faviasCover`.

У каждого проекта есть поля:

- `title` — название карточки и страницы проекта.
- `type` — тип проекта.
- `description` — короткий текст в карточке.
- `details` — основной текст внутри попапа.
- `imageClass` или `image` — CSS-обложка или реальная картинка.

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
