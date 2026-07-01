<script setup>
import { onBeforeUnmount, ref } from "vue";

const email = "irina_zheleznova97@mail.ru";
const hhUrl =
    "https://hh.ru/resume/9ccb1af5ff10b9184e0039ed1f364f434f4f75";

const copyMessage = ref("");
const activeProject = ref(null);
let copyMessageTimer;

const experience = [
    {
        period: "2025",
        company: "Дополнительный проект",
        note: "(в разработке)",
        tasks: [
            "Проектирование мобильного приложения с нуля в составе команды.",
            "Разработка интерфейсов, создание User Flow, UI Kit, библиотеки компонентов и дизайн-системы в Figma.",
        ],
    },
    {
        period: "2024–2026",
        company: "Московский Кредитный Банк",
        tasks: [
            "Разработка цифровых материалов для мобильного приложения, веб-ресурсов и внутренних коммуникаций.",
            "Работа в Figma: библиотеки компонентов, шаблоны и переиспользуемые элементы.",
            "Поддержка единого визуального стиля и бренд-гайдлайнов.",
            "Взаимодействие с внутренними заказчиками.",
            "Внедрение AI-инструментов в рабочие процессы.",
        ],
    },
    {
        period: "2023–2024",
        company: "МАКС-Жизнь",
        note: "Страховая компания",
        tasks: [
            "Разработка и поддержка дизайна интерфейсов и отдельных экранов для личного кабинета.",
            "Создание лендингов и промо-страниц, адаптация дизайна под различные устройства.",
            "Проектирование пользовательских интерфейсов и цифровых маркетинговых материалов.",
            "Анализ рынка, конкурентов и визуальных решений.",
        ],
    },
];

const skillGroups = [
    {
        title: "Tools",
        skills: ["Figma", "Adobe Illustrator", "Adobe Photoshop", "ChatGPT", "Gemini"],
    },
    {
        title: "Design",
        skills: [
            "UI/UX Design",
            "UI Kit",
            "Mobile Design",
            "Web Design",
            "Design System",
            "Adaptive Design",
            "User Flow",
            "Wireframes",
            "Прототипирование",
            "Branding",
            "Identity",
        ],
    },
];

const projects = [
    {
        title: "Редизайн приложения ресторана",
        shortTitle: "Restaurant app",
        accent: "red",
        figmaUrl: "",
    },
    {
        title: "Мобильное приложение",
        shortTitle: "Mobile app",
        accent: "blue",
        figmaUrl: "",
    },
    {
        title: "Личный кабинет",
        shortTitle: "Web service",
        accent: "green",
        figmaUrl: "",
    },
    {
        title: "Новый проект",
        shortTitle: "Concept",
        accent: "violet",
        figmaUrl: "",
    },
];

const researchTags = [
    "UX Research",
    "User Flow",
    "Wireframes",
    "UI Kit",
    "UI Design",
    "Prototype",
];

const problems = [
    ["Визуальный шум", "Интерфейс перегружен яркими цветами, баннерами и фоновыми изображениями."],
    ["Сложный выбор товара", "Карточки содержат минимум информации и усложняют принятие решения."],
    ["Неудобная структура", "Для просмотра состава блюда необходимо переходить между экранами."],
    ["Лишние элементы", "Часть функций главного экрана не используется или ведет на пустые страницы."],
];

function openProject(project) {
    activeProject.value = project;
    document.body.classList.add("case-open");
}

function closeProject() {
    activeProject.value = null;
    document.body.classList.remove("case-open");
}

function getFigmaEmbedUrl(url) {
    return `https://www.figma.com/embed?embed_host=share&url=${encodeURIComponent(url)}`;
}

async function copyContact(value, message) {
    try {
        await navigator.clipboard.writeText(value);
    } catch {
        const input = document.createElement("textarea");
        input.value = value;
        input.style.position = "fixed";
        input.style.opacity = "0";
        document.body.appendChild(input);
        input.select();
        document.execCommand("copy");
        input.remove();
    }

    copyMessage.value = message;
    window.clearTimeout(copyMessageTimer);
    copyMessageTimer = window.setTimeout(() => {
        copyMessage.value = "";
    }, 2200);
}

function handleKeydown(event) {
    if (event.key === "Escape" && activeProject.value) closeProject();
}

window.addEventListener("keydown", handleKeydown);

onBeforeUnmount(() => {
    window.removeEventListener("keydown", handleKeydown);
    window.clearTimeout(copyMessageTimer);
    document.body.classList.remove("case-open");
});
</script>

<template>
    <main class="portfolio">
        <section class="intro" aria-labelledby="intro-title">
            <div class="portrait" role="img" aria-label="Фото Ирины Железновой">
                <span>ИЖ</span>
            </div>

            <div class="intro__content">
                <span class="role-badge">UI/UX DESIGNER</span>
                <h1 id="intro-title">ИРИНА ЖЕЛЕЗНОВА</h1>
                <p class="intro__lead">Проектирую понятные интерфейсы для веб и мобильных продуктов</p>
                <p class="intro__meta">28 лет&nbsp; • &nbsp;Москва&nbsp; • &nbsp;Удаленная работа</p>

                <nav class="contact-list" aria-label="Контакты">
                    <button type="button" @click="copyContact(email, 'Email скопирован')">E-mail</button>
                    <a href="https://t.me/kirabirl" target="_blank" rel="noreferrer">Telegram</a>
                    <a :href="hhUrl" target="_blank" rel="noreferrer">HeadHunter</a>
                </nav>
            </div>
        </section>

        <section class="content-section" aria-labelledby="about-title">
            <h2 id="about-title">Обо мне</h2>
            <p>
                Более 3 лет я работаю в digital дизайне. Постепенно поняла, что больше всего мне
                нравится проектировать интерфейсы и продумывать пользовательский опыт. Сейчас
                развиваюсь в направлении UI/UX, создаю собственные проекты и постоянно совершенствую
                свои навыки. Люблю сначала разобраться в задаче, а потом искать простые и удобные
                решения для пользователей.
            </p>
            <p>В свободное время изучаю немецкий язык и люблю компьютерные игры.</p>
        </section>

        <details class="resume-section">
            <summary>Опыт работы</summary>
            <div class="experience-list">
                <article v-for="job in experience" :key="job.period + job.company" class="experience-card">
                    <div class="experience-card__heading">
                        <span>{{ job.period }}</span>
                        <strong>{{ job.company }}</strong>
                        <small v-if="job.note">{{ job.note }}</small>
                    </div>
                    <ul>
                        <li v-for="task in job.tasks" :key="task">{{ task }}</li>
                    </ul>
                </article>
            </div>
        </details>

        <details class="resume-section">
            <summary>Навыки</summary>
            <div class="skills-grid">
                <article v-for="group in skillGroups" :key="group.title" class="skill-panel">
                    <h3>{{ group.title }}</h3>
                    <div class="tag-list">
                        <span v-for="skill in group.skills" :key="skill">{{ skill }}</span>
                    </div>
                </article>
            </div>
        </details>

        <section class="portfolio-section" aria-labelledby="portfolio-title">
            <h2 id="portfolio-title">Портфолио</h2>
            <div class="project-grid">
                <button
                    v-for="project in projects"
                    :key="project.title"
                    type="button"
                    class="project-tile"
                    @click="openProject(project)"
                >
                    <span class="project-tile__preview" :class="`project-tile__preview--${project.accent}`">
                        <span>{{ project.shortTitle }}</span>
                    </span>
                    <span class="project-tile__title">{{ project.title }}</span>
                </button>
            </div>
        </section>
    </main>

    <Transition name="toast">
        <div v-if="copyMessage" class="copy-toast" role="status" aria-live="polite">
            {{ copyMessage }}
        </div>
    </Transition>

    <Teleport to="body">
        <div
            v-if="activeProject"
            class="case-page"
            role="dialog"
            aria-modal="true"
            aria-label="Описание проекта"
            @click.self="closeProject"
        >
            <div class="case-modal">
                <button type="button" class="case-close" @click="closeProject" aria-label="Закрыть проект">
                    <span aria-hidden="true">×</span>
                </button>

                <article class="case-study">
                <header class="case-hero">
                    <h2>{{ activeProject.title }}</h2>
                    <p>
                        Личный UX/UI-проект по редизайну существующего приложения немецкого ресторана.
                        Цель — улучшить пользовательский опыт и сохранить его функциональность.
                    </p>
                    <h3>Моя роль</h3>
                    <div class="tag-list">
                        <span v-for="tag in researchTags" :key="tag">{{ tag }}</span>
                    </div>
                </header>

                <section class="case-section">
                    <h3>Исследование</h3>
                    <p>
                        Был проведен анализ существующего приложения и опрос потенциальных
                        пользователей, чтобы подтвердить найденные проблемы и определить основные
                        пользовательские сценарии.
                    </p>
                    <h4>Методы исследования</h4>
                    <div class="tag-list tag-list--wide">
                        <span>Анализ существующего приложения</span>
                        <span>Анализ пользовательских сценариев</span>
                        <span>Интервью с пользователями</span>
                        <span>Анализ конкурентов</span>
                    </div>
                </section>

                <section class="case-section">
                    <h4>Исходное приложение</h4>
                    <div class="screen-board screen-board--old" aria-label="Место для экранов исходного приложения">
                        <div v-for="index in 6" :key="index" class="phone-mock phone-mock--old">
                            <span></span><span></span><span></span><span></span>
                        </div>
                    </div>
                </section>

                <section class="case-section">
                    <h4>Основные проблемы</h4>
                    <div class="problem-list">
                        <div v-for="problem in problems" :key="problem[0]" class="problem-row">
                            <strong>{{ problem[0] }}</strong>
                            <p>{{ problem[1] }}</p>
                        </div>
                    </div>
                </section>

                <section class="case-section case-section--spaced">
                    <h3>User Flow</h3>
                    <p>На основе результатов исследования был переработан пользовательский сценарий и упрощен путь оформления заказа.</p>
                    <div class="diagram-board flow-board" aria-label="Место для схемы User Flow">
                        <div v-for="index in 7" :key="index" class="flow-column">
                            <i></i><span></span><span></span><b></b>
                        </div>
                    </div>
                </section>

                <section class="case-section case-section--spaced">
                    <h3>Wireframes</h3>
                    <p>После определения структуры были созданы wireframes для проверки компоновки экранов и пользовательских сценариев.</p>
                    <div class="diagram-board wireframe-board" aria-label="Место для вайрфреймов">
                        <div v-for="index in 12" :key="index" class="wireframe-phone">
                            <i></i><span></span><span></span><span></span>
                        </div>
                    </div>
                </section>

                <section class="case-section case-section--spaced">
                    <h3>UI Kit</h3>
                    <p>Для ускорения разработки и обеспечения единообразия интерфейса была создана дизайн-система.</p>
                    <div class="diagram-board kit-board" aria-label="Место для UI Kit">
                        <div class="kit-group"><i></i><i></i><i></i><i></i></div>
                        <div class="kit-group kit-group--accent"><i></i><i></i><i></i></div>
                        <div class="kit-sidebar"></div>
                    </div>
                </section>

                <section class="case-section case-section--spaced">
                    <h3>Финальный дизайн</h3>
                    <iframe
                        v-if="activeProject.figmaUrl"
                        class="figma-embed"
                        :src="getFigmaEmbedUrl(activeProject.figmaUrl)"
                        allowfullscreen
                        title="Интерактивный макет Figma"
                    ></iframe>
                    <div v-else class="diagram-board final-board" aria-label="Место для финальных экранов">
                        <div v-for="index in 10" :key="index" class="final-phone">
                            <i></i><span></span><span></span><b></b>
                        </div>
                    </div>
                    <h4>Что изменилось</h4>
                    <div class="tag-list tag-list--wide">
                        <span>Упростила структуру навигации</span>
                        <span>Добавила визуализацию товаров</span>
                        <span>Переработала карточку блюда</span>
                        <span>Сократила количество лишних действий</span>
                        <span>Снизила визуальную нагрузку</span>
                    </div>
                </section>
                </article>
            </div>
        </div>
    </Teleport>
</template>
