<script setup>
import { nextTick, onBeforeUnmount, ref, watch } from "vue";
import AssetFrame from "./components/AssetFrame.vue";
import ProjectModal from "./components/ProjectModal.vue";
import { experience, profile, projects, skillGroups } from "./data.js";

const activeProject = ref(null);
const toast = ref("");

const profileClickCount = ref(0);
const isCatMode = ref(false);
const catVideoRef = ref(null);

let toastTimer;
let profileClickResetTimer;

const catVideo = `${import.meta.env.BASE_URL}portfolio/beep-cat.mp4`;

async function copyEmail() {
    try {
        await navigator.clipboard.writeText(profile.email);
    } catch {
        const field = document.createElement("textarea");
        field.value = profile.email;
        field.style.position = "fixed";
        field.style.opacity = "0";
        document.body.appendChild(field);
        field.select();
        document.execCommand("copy");
        field.remove();
    }

    toast.value = "Email скопирован";
    window.clearTimeout(toastTimer);
    toastTimer = window.setTimeout(() => {
        toast.value = "";
    }, 2200);
}

async function activateCatMode() {
    isCatMode.value = true;
    profileClickCount.value = 0;

    window.clearTimeout(profileClickResetTimer);

    await nextTick();

    if (catVideoRef.value) {
        catVideoRef.value.currentTime = 0;
        catVideoRef.value.volume = 0.22;

        try {
            await catVideoRef.value.play();
        } catch {
            // Если браузер заблокирует звук, видео всё равно останется видимым.
        }
    }
}

function deactivateCatMode() {
    isCatMode.value = false;
    profileClickCount.value = 0;
}

function handleProfileClick() {
    if (isCatMode.value) return;

    profileClickCount.value += 1;

    window.clearTimeout(profileClickResetTimer);
    profileClickResetTimer = window.setTimeout(() => {
        profileClickCount.value = 0;
    }, 5000);

    if (profileClickCount.value >= 5) {
        activateCatMode();
    }
}

function openProject(project) {
    activeProject.value = project;
}

function closeProject() {
    activeProject.value = null;
}

function handleKeydown(event) {
    if (event.key === "Escape") closeProject();
}

watch(activeProject, (project) => {
    document.body.classList.toggle("modal-open", Boolean(project));
});

window.addEventListener("keydown", handleKeydown);

onBeforeUnmount(() => {
    window.removeEventListener("keydown", handleKeydown);
    window.clearTimeout(toastTimer);
    window.clearTimeout(profileClickResetTimer);
    document.body.classList.remove("modal-open");
});
</script>

<template>
    <main class="portfolio-shell">
        <section class="hero-block">
            <header class="profile-header">
                <button
                    class="profile-photo profile-photo-button"
                    type="button"
                    aria-label="Фото профиля"
                    @click="handleProfileClick"
                >
                    <video
                        v-if="isCatMode"
                        ref="catVideoRef"
                        class="profile-cat-video"
                        :src="catVideo"
                        playsinline
                        @ended="deactivateCatMode"
                    />

                    <AssetFrame
                        v-else
                        :src="profile.photo"
                        alt="Ирина Железнова"
                        ratio="square"
                    />
                </button>

                <div class="profile-copy">
                    <span class="role-pill">{{ profile.role }}</span>
                    <h1>{{ profile.name }}</h1>
                    <p>{{ profile.meta }}</p>

                    <nav class="contact-row" aria-label="Контакты">
                        <button type="button" @click="copyEmail">E-mail</button>
                        <a
                            :href="profile.telegram"
                            target="_blank"
                            rel="noreferrer"
                            >Telegram</a
                        >
                        <a :href="profile.cv" target="_blank" rel="noreferrer"
                            >CV</a
                        >
                    </nav>
                </div>
            </header>

            <section class="about-section" aria-labelledby="about-title">
                <h2 id="about-title">Обо мне</h2>
                <p>
                    Более 3 лет работаю в digital дизайне. Постепенно поняла,
                    что больше всего мне нравится проектировать интерфейсы и
                    продумывать пользовательский опыт. Сейчас развиваюсь в
                    направлении UI/UX, создаю собственные проекты и постоянно
                    совершенствую свои навыки. Люблю сначала разобраться в
                    задаче, а потом искать простые и удобные решения для
                    пользователей.
                </p>
                <p>
                    В свободное время я изучаю немецкий язык и люблю
                    компьютерные игры.
                </p>
            </section>
        </section>

        <details class="disclosure">
            <summary>Опыт работы</summary>

            <div class="experience-table">
                <article
                    v-for="job in experience"
                    :key="job.period + job.company"
                    class="experience-row"
                >
                    <div class="experience-heading">
                        <span>{{ job.period }}</span>
                        <strong>{{ job.company }}</strong>
                        <small v-if="job.note">{{ job.note }}</small>
                    </div>

                    <ul>
                        <li v-for="task in job.tasks" :key="task">
                            {{ task }}
                        </li>
                    </ul>
                </article>
            </div>
        </details>

        <details class="disclosure">
            <summary>Навыки</summary>

            <div class="skills-table">
                <article v-for="group in skillGroups" :key="group.title">
                    <h3>{{ group.title }}</h3>

                    <div class="tag-list">
                        <span v-for="skill in group.skills" :key="skill">{{
                            skill
                        }}</span>
                    </div>
                </article>
            </div>
        </details>

        <section class="portfolio-section" aria-labelledby="portfolio-title">
            <h2 id="portfolio-title">Портфолио</h2>

            <div class="project-grid">
                <button
                    v-for="project in projects"
                    :key="project.id"
                    type="button"
                    class="project-card"
                    @click="openProject(project)"
                >
                    <AssetFrame
                        :src="project.cover"
                        :alt="project.title"
                        ratio="cover"
                    />
                    <span>{{ project.title }}</span>
                </button>
            </div>
        </section>
    </main>

    <Transition name="toast">
        <div v-if="toast" class="toast" role="status" aria-live="polite">
            {{ toast }}
        </div>
    </Transition>

    <Teleport to="body">
        <ProjectModal
            v-if="activeProject"
            :project="activeProject"
            @close="closeProject"
        />
    </Teleport>
</template>
