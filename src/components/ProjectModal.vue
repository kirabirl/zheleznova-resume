<script setup>
import AssetFrame from './AssetFrame.vue'

defineProps({ project: { type: Object, required: true } })
defineEmits(['close'])
</script>

<template>
  <div class="modal-layer" role="dialog" aria-modal="true" :aria-label="project.title" @click.self="$emit('close')">
    <article class="case-modal">
      <button class="case-modal__close" type="button" aria-label="Закрыть проект" @click="$emit('close')">×</button>

      <div class="case-content">
        <header class="case-header">
          <h2>{{ project.heading }}</h2>
          <p v-if="project.intro">{{ project.intro }}</p>
          <template v-if="project.tags">
            <h3>Моя роль</h3>
            <div class="tag-list">
              <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
            </div>
          </template>
        </header>

        <section v-for="section in project.sections" :key="section.title" class="case-section">
          <h3>{{ section.title }}</h3>
          <p v-if="section.text">{{ section.text }}</p>
          <p v-for="paragraph in section.paragraphs" :key="paragraph">{{ paragraph }}</p>

          <AssetFrame
            v-if="section.image"
            :src="section.image"
            :alt="`${project.title}: ${section.title}`"
            :ratio="section.ratio"
          />

          <div v-if="section.problems" class="problem-list">
            <div v-for="problem in section.problems" :key="problem[0]" class="problem-row">
              <strong>{{ problem[0] }}</strong>
              <p>{{ problem[1] }}</p>
            </div>
          </div>

          <div v-if="section.steps" class="process-flow">
            <template v-for="(step, index) in section.steps" :key="step">
              <span>{{ step }}</span><i v-if="index < section.steps.length - 1" aria-hidden="true">→</i>
            </template>
          </div>

          <div v-if="section.gallery" class="case-gallery">
            <figure v-for="item in section.gallery" :key="item[0]">
              <figcaption>{{ item[0] }}</figcaption>
              <AssetFrame :src="item[1]" :alt="item[0]" ratio="portrait" />
            </figure>
          </div>

          <template v-if="section.subtitle">
            <h4>{{ section.subtitle }}</h4>
            <div class="tag-list">
              <span v-for="tag in section.tags" :key="tag">{{ tag }}</span>
            </div>
          </template>
        </section>
      </div>
    </article>
  </div>
</template>
