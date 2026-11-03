<script>
import ProjectCard from './ProjectCard.vue'

/**
 * Список проектов. Получает уже отфильтрованный массив через props
 * и выводит для каждого элемента ProjectCard с помощью v-for.
 */
export default {
  name: 'ProjectList',
  components: { ProjectCard },
  props: {
    projects: {
      type: Array,
      required: true,
    },
    total: {
      type: Number,
      required: true,
    },
    favoriteIds: {
      type: Array,
      required: true,
    },
    query: {
      type: String,
      default: '',
    },
  },
  emits: ['toggle-favorite', 'reset-filter'],
  data() {
    return {
      // вид отображения: сетка или список
      layout: 'grid',
    }
  },
  methods: {
    isFavorite(id) {
      return this.favoriteIds.includes(id)
    },
    setLayout(layout) {
      this.layout = layout
    },
  },
}
</script>

<template>
  <section class="list">
    <div class="toolbar">
      <p class="counter">
        Показано <strong>{{ projects.length }}</strong> из {{ total }}
        <template v-if="favoriteIds.length"> · в избранном: {{ favoriteIds.length }}</template>
      </p>
      <div class="switch" role="group" aria-label="Вид списка">
        <button
          type="button"
          class="switch-btn"
          :class="{ active: layout === 'grid' }"
          :aria-pressed="layout === 'grid'"
          @click="setLayout('grid')"
        >
          Сетка
        </button>
        <button
          type="button"
          class="switch-btn"
          :class="{ active: layout === 'list' }"
          :aria-pressed="layout === 'list'"
          @click="setLayout('list')"
        >
          Список
        </button>
      </div>
    </div>

    <!-- проектов нет вообще -->
    <div v-if="total === 0" class="empty">
      <h3 class="empty-title">Проектов пока нет</h3>
      <p class="empty-text">Скоро здесь появятся мои работы.</p>
    </div>

    <!-- по запросу ничего не найдено -->
    <div v-else-if="projects.length === 0" class="empty">
      <h3 class="empty-title">По запросу «{{ query }}» ничего не найдено</h3>
      <p class="empty-text">Попробуйте другое название или технологию, например «Vue» или «Figma».</p>
      <button type="button" class="btn btn-secondary" @click="$emit('reset-filter')">Сбросить поиск</button>
    </div>

    <!-- список карточек -->
    <div v-else class="grid" :class="{ 'grid-list': layout === 'list' }">
      <ProjectCard
        v-for="project in projects"
        :key="project.id"
        :project="project"
        :is-favorite="isFavorite(project.id)"
        @toggle-favorite="$emit('toggle-favorite', $event)"
      />
    </div>
  </section>
</template>

<style scoped>
.toolbar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 20px;
}

/* Body/Small */
.counter {
  margin: 0;
  font-size: 14px;
  line-height: 20px;
  color: var(--neutral-600);
}

.counter strong {
  color: var(--neutral-900);
}

.switch {
  display: inline-flex;
  padding: 4px;
  border: 1px solid var(--neutral-200);
  border-radius: var(--radius-control);
  background-color: var(--neutral-0);
}

/* Nav Item: Default / Active */
.switch-btn {
  min-height: 36px;
  padding: 6px 14px;
  border: none;
  border-radius: 8px;
  background: transparent;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  color: var(--neutral-600);
  cursor: pointer;
}

.switch-btn.active {
  background-color: var(--brand-primary-subtle);
  color: var(--brand-primary);
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 24px;
}

.grid-list {
  grid-template-columns: 1fr;
  max-width: 640px;
}

/* Empty State */
.empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 48px 24px;
  border: 1.5px dashed var(--neutral-300);
  border-radius: var(--radius-card);
  background-color: var(--neutral-0);
  text-align: center;
}

.empty-title {
  margin: 0;
  font-family: var(--font-heading);
  font-size: 20px;
  font-weight: 700;
  line-height: 28px;
}

.empty-text {
  margin: 0 0 12px;
  font-size: 14px;
  line-height: 20px;
  color: var(--neutral-600);
}
</style>
