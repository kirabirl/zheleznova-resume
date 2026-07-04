<script setup>
import { ref } from "vue";
import AssetFrame from "./AssetFrame.vue";

defineProps({ project: { type: Object, required: true } });
const emit = defineEmits(["close"]);

const zoomedImage = ref(null);

function normalizeGalleryItem(item) {
    if (Array.isArray(item)) {
        return {
            title: item[0],
            image: item[1],
            ratio: "natural",
        };
    }

    return {
        title: item.title,
        image: item.image,
        ratio: item.ratio || "natural",
    };
}

function openImage(src, alt) {
    zoomedImage.value = { src, alt };
}

function closeImage() {
    zoomedImage.value = null;
}

function closeProject() {
    zoomedImage.value = null;
    emit("close");
}
</script>

<template>
    <div
        class="modal-layer"
        role="dialog"
        aria-modal="true"
        :aria-label="project.title"
        @click.self="closeProject"
    >
        <article class="case-modal">
            <button
                class="case-modal__close"
                type="button"
                aria-label="Закрыть проект"
                @click="closeProject"
            >
                ×
            </button>

            <div class="case-content">
                <header class="case-header">
                    <h2>{{ project.heading }}</h2>
                    <p v-if="project.intro">{{ project.intro }}</p>

                    <template v-if="project.tags">
                        <h3>Моя роль</h3>
                        <div class="tag-list">
                            <span v-for="tag in project.tags" :key="tag">{{
                                tag
                            }}</span>
                        </div>
                    </template>
                </header>

                <section
                    v-for="section in project.sections"
                    :key="section.title"
                    class="case-section"
                    :class="{
                        'case-section--spoiler': section.title === 'Спойлер',
                    }"
                >
                    <h3>{{ section.title }}</h3>

                    <p v-if="section.text">{{ section.text }}</p>
                    <p v-for="paragraph in section.paragraphs" :key="paragraph">
                        {{ paragraph }}
                    </p>

                    <button
                        v-if="section.image"
                        type="button"
                        class="image-open-button"
                        :aria-label="`Открыть изображение: ${project.title}, ${section.title}`"
                        @click="
                            openImage(
                                section.image,
                                `${project.title}: ${section.title}`,
                            )
                        "
                    >
                        <AssetFrame
                            :src="section.image"
                            :alt="`${project.title}: ${section.title}`"
                            :ratio="section.ratio || 'natural'"
                        />
                    </button>

                    <div v-if="section.problems" class="problem-list">
                        <div
                            v-for="problem in section.problems"
                            :key="problem[0]"
                            class="problem-row"
                        >
                            <strong>{{ problem[0] }}</strong>
                            <p>{{ problem[1] }}</p>
                        </div>
                    </div>

                    <div v-if="section.steps" class="process-flow">
                        <template
                            v-for="(step, index) in section.steps"
                            :key="step"
                        >
                            <span>{{ step }}</span>
                            <i
                                v-if="index < section.steps.length - 1"
                                aria-hidden="true"
                                >→</i
                            >
                        </template>
                    </div>

                    <div v-if="section.gallery" class="case-gallery">
                        <figure
                            v-for="rawItem in section.gallery"
                            :key="normalizeGalleryItem(rawItem).title"
                            :class="`case-gallery__item case-gallery__item--${normalizeGalleryItem(rawItem).ratio}`"
                        >
                            <figcaption>
                                {{ normalizeGalleryItem(rawItem).title }}
                            </figcaption>

                            <button
                                type="button"
                                class="image-open-button"
                                :aria-label="`Открыть изображение: ${normalizeGalleryItem(rawItem).title}`"
                                @click="
                                    openImage(
                                        normalizeGalleryItem(rawItem).image,
                                        normalizeGalleryItem(rawItem).title,
                                    )
                                "
                            >
                                <AssetFrame
                                    :src="normalizeGalleryItem(rawItem).image"
                                    :alt="normalizeGalleryItem(rawItem).title"
                                    :ratio="normalizeGalleryItem(rawItem).ratio"
                                />
                            </button>
                        </figure>
                    </div>

                    <template v-if="section.subtitle">
                        <h4>{{ section.subtitle }}</h4>
                        <div class="tag-list">
                            <span v-for="tag in section.tags" :key="tag">{{
                                tag
                            }}</span>
                        </div>
                    </template>
                </section>
            </div>
        </article>

        <div v-if="zoomedImage" class="image-lightbox" @click.self="closeImage">
            <button
                class="image-lightbox__close"
                type="button"
                aria-label="Закрыть изображение"
                @click="closeImage"
            >
                ×
            </button>

            <img :src="zoomedImage.src" :alt="zoomedImage.alt" />
        </div>
    </div>
</template>
