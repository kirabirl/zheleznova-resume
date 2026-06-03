<script setup>
import { computed, onBeforeUnmount, ref, watch } from 'vue'

const links = [
  { label: 'E-mail', href: 'mailto:hello@example.com' },
  { label: 'LinkedIn', href: 'https://www.linkedin.com/' },
  { label: 'Telegram', href: 'https://t.me/' },
  { label: 'CV', href: '#' },
]

const skillGroups = [
  {
    title: 'Исследования',
    lines: [
      'Пользовательские сценарии • интервью • анализ задач',
      'Проверка гипотез • структура продукта • прототипирование',
      'Сбор требований • карта опыта • работа с обратной связью',
    ],
  },
  {
    title: 'Дизайн',
    lines: [
      'Интерфейсы • дизайн-системы • адаптивные макеты',
      'Визуальная коммуникация • сетки • компоненты',
    ],
  },
  {
    title: 'Инструменты',
    lines: [
      'Figma • FigJam • Miro • Adobe • Tilda • Readymag',
      'HTML • CSS • базовая работа с веб-средой',
    ],
  },
]

const projects = [
  {
    title: 'Проект 01',
    type: 'website',
    description: 'Короткое описание проекта, роли и основной задачи.',
    details:
      'Здесь будет подробная страница проекта: вводная, задача, процесс, ключевые решения и результат. Пока это нейтральный текст-заглушка, который можно заменить на реальный кейс.',
    imageClass: 'cover-favias',
  },
  {
    title: 'Проект 02',
    type: 'mobile app',
    description: 'Описание мобильного интерфейса или продуктовой концепции.',
    details:
      'На этой странице можно показать контекст проекта, несколько экранов, что было сделано и какую задачу решал дизайн.',
    imageClass: 'cover-volchok',
  },
  {
    title: 'Проект 03',
    type: 'service',
    description: 'Заглушка для сервиса, лендинга или внутреннего продукта.',
    details:
      'Тут будет развернутое описание: цель, аудитория, структура решения, визуальная система и финальные материалы.',
    imageClass: 'cover-msp',
  },
  {
    title: 'Проект 04',
    type: 'website',
    description: 'Карточка для веб-проекта с будущими изображениями.',
    details:
      'Попап подходит для кейса с большим количеством визуалов: обложка, текстовые блоки, список работ и дополнительные ссылки.',
    imageClass: 'cover-stt',
  },
  {
    title: 'Проект 05',
    type: 'concept',
    description: 'Заглушка для концепта, исследования или командной работы.',
    details:
      'В дальнейшем здесь можно описать вклад Иры, ограничения проекта, этапы работы и итоговый результат.',
    imageClass: 'cover-mkb',
  },
  {
    title: 'Проект 06',
    type: 'web',
    description: 'Место под еще один проект в галерее.',
    details:
      'Это временное наполнение. Когда появятся реальные материалы, достаточно заменить текст и добавить изображения в массив проекта.',
    imageClass: 'cover-ice',
  },
]

const activeProjectIndex = ref(null)

const activeProject = computed(() =>
  activeProjectIndex.value === null ? null : projects[activeProjectIndex.value],
)

function openProject(index) {
  activeProjectIndex.value = index
}

function closeProject() {
  activeProjectIndex.value = null
}

function handleKeydown(event) {
  if (event.key === 'Escape') {
    closeProject()
  }
}

watch(activeProject, (project) => {
  document.body.classList.toggle('modal-open', Boolean(project))

  if (project) {
    window.addEventListener('keydown', handleKeydown)
  } else {
    window.removeEventListener('keydown', handleKeydown)
  }
})

onBeforeUnmount(() => {
  document.body.classList.remove('modal-open')
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<template>
  <main class="page">
    <section class="hero" aria-labelledby="hero-title">
      <div class="hero__photo" aria-hidden="true"></div>

      <div class="hero__content">
        <p class="eyebrow">UX/UI designer</p>
        <h1 id="hero-title">Ира Железнова</h1>
        <p class="lead">
          Дизайнер интерфейсов и цифровых продуктов. Здесь будет короткое описание опыта,
          подхода к работе и направлений, с которыми хочется ассоциировать портфолио.
        </p>

        <nav class="links" aria-label="Контакты">
          <a v-for="link in links" :key="link.label" :href="link.href" target="_blank" rel="noreferrer">
            {{ link.label }}
          </a>
        </nav>

        <p class="location">Город / формат работы</p>
      </div>
    </section>

    <section class="goal" aria-label="Коротко">
      <p>
        <strong>Коротко:</strong> этот блок можно использовать для личного позиционирования,
        текущего фокуса или пары предложений о том, какие задачи интересны дизайнеру.
      </p>
    </section>

    <section class="about" aria-labelledby="about-title">
      <h2 id="about-title">UX/UI дизайнер</h2>
      <p>
        Нейтральный текст-заглушка для блока о себе. Здесь можно описать опыт,
        сильные стороны, рабочий процесс, типы проектов и то, как дизайнер
        взаимодействует с командами и заказчиками.
      </p>

      <div class="doc-links">
        <a href="#">Резюме</a>
        <a href="#">Рекомендации</a>
        <a href="#">Дополнительная ссылка</a>
      </div>

      <ul>
        <li>Здесь может быть актуальная роль, формат занятости или специализация.</li>
        <li>Здесь может быть второй факт: образование, опыт, индустрии или интересы.</li>
      </ul>
    </section>

    <section class="skills" aria-labelledby="skills-title">
      <h2 id="skills-title">Навыки</h2>
      <div class="skills__grid">
        <article v-for="group in skillGroups" :key="group.title" class="skill-card">
          <h3>{{ group.title }}</h3>
          <p v-for="line in group.lines" :key="line">{{ line }}</p>
        </article>
      </div>
    </section>

    <section class="projects" aria-labelledby="projects-title">
      <div class="section-head">
        <p class="eyebrow">Мои работы</p>
        <h2 id="projects-title">Проекты</h2>
      </div>

      <div class="gallery">
        <button
          v-for="(project, index) in projects"
          :key="project.title"
          type="button"
          class="project-card"
          @click="openProject(index)"
        >
          <span class="project-card__image" :class="project.imageClass">
            <img v-if="project.image" :src="project.image" :alt="project.title" />
            <span v-else>{{ project.title }}</span>
          </span>
          <span class="project-card__body">
            <span class="project-card__meta">{{ project.type }}</span>
            <strong>{{ project.title }}</strong>
            <span>{{ project.description }}</span>
          </span>
        </button>
      </div>
    </section>
  </main>

  <Teleport to="body">
    <div v-if="activeProject" class="project-modal" role="dialog" aria-modal="true">
      <button class="project-modal__backdrop" type="button" aria-label="Закрыть проект" @click="closeProject"></button>

      <article class="project-modal__panel">
        <button class="back-button" type="button" @click="closeProject">
          <span aria-hidden="true">←</span>
          Назад
        </button>

        <div class="project-modal__cover" :class="activeProject.imageClass">
          <img v-if="activeProject.image" :src="activeProject.image" :alt="activeProject.title" />
          <span v-else>{{ activeProject.title }}</span>
        </div>

        <div class="project-modal__content">
          <p class="eyebrow">{{ activeProject.type }}</p>
          <h2>{{ activeProject.title }}</h2>
          <p class="lead">{{ activeProject.description }}</p>
          <p>{{ activeProject.details }}</p>

          <div class="project-modal__grid">
            <section>
              <h3>Задача</h3>
              <p>Краткое описание цели проекта и контекста, который будет заполнен позже.</p>
            </section>
            <section>
              <h3>Роль</h3>
              <p>Здесь можно указать вклад дизайнера, этапы работы и используемые инструменты.</p>
            </section>
            <section>
              <h3>Результат</h3>
              <p>Место для итогов, ссылок, метрик или описания готового решения.</p>
            </section>
          </div>
        </div>
      </article>
    </div>
  </Teleport>
</template>
