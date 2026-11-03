<script>
import heartIcon from '../assets/icons/icon-heart.svg'
import heartFilledIcon from '../assets/icons/icon-heart-filled.svg'
import arrowIcon from '../assets/icons/icon-arrow-right.svg'

/**
 * Карточка проекта (Project Card из ЛР №17).
 * Данные проекта приходят через props, нажатие на «сердце» передаётся родителю событием toggle-favorite.
 */
export default {
  name: 'ProjectCard',
  props: {
    project: {
      type: Object,
      required: true,
    },
    isFavorite: {
      type: Boolean,
      default: false,
    },
  },
  emits: ['toggle-favorite'],
  data() {
    return {
      // раскрыто ли полное описание
      isExpanded: false,
      heartIcon,
      heartFilledIcon,
      arrowIcon,
    }
  },
  methods: {
    onFavoriteClick() {
      this.$emit('toggle-favorite', this.project.id)
    },
    toggleDescription() {
      this.isExpanded = !this.isExpanded
    },
  },
}
</script>

<template>
  <article class="card">
    <div class="cover">
      <img :src="project.image" :alt="`Обложка проекта «${project.title}»`" class="image" />
      <button
        type="button"
        class="favorite"
        :class="{ 'favorite-on': isFavorite }"
        :aria-pressed="isFavorite"
        :aria-label="isFavorite ? 'Убрать из избранного' : 'Добавить в избранное'"
        @click="onFavoriteClick"
      >
        <span
          class="icon"
          :style="{ '--icon': `url(${isFavorite ? heartFilledIcon : heartIcon})` }"
          aria-hidden="true"
        ></span>
      </button>
    </div>

    <div class="body">
      <ul v-if="project.tags.length" class="tags">
        <li v-for="tag in project.tags" :key="tag" class="tag">{{ tag }}</li>
      </ul>
      <h3 class="title">{{ project.title }}</h3>
      <p class="description" :class="{ collapsed: !isExpanded }">{{ project.description }}</p>
      <div class="footer">
        <button type="button" class="link" :aria-expanded="isExpanded" @click="toggleDescription">
          {{ isExpanded ? 'Свернуть' : 'Подробнее' }}
          <span
            class="icon"
            :class="{ rotated: isExpanded }"
            :style="{ '--icon': `url(${arrowIcon})` }"
            aria-hidden="true"
          ></span>
        </button>
        <span class="meta">{{ project.year }} · фриланс</span>
      </div>
    </div>
  </article>
</template>

<style scoped>
/* Project Card: вертикальный Auto Layout, тело — отступ 24, gap 12, радиус 20 */
.card {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
  border: 1px solid var(--neutral-200);
  border-radius: var(--radius-card);
  background-color: var(--neutral-0);
  box-shadow: var(--shadow-card);
  transition: transform 0.2s, box-shadow 0.2s;
}

/* State: Hover */
.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-card-hover);
}

.card:hover .title {
  color: var(--brand-primary);
}

.cover {
  position: relative;
}

.image {
  display: block;
  width: 100%;
  aspect-ratio: 9 / 5;
  object-fit: cover;
}

/* Favorite: Off / On */
.favorite {
  position: absolute;
  top: 12px;
  right: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 50%;
  background-color: var(--neutral-0);
  box-shadow: var(--shadow-card);
  color: var(--neutral-900);
  cursor: pointer;
}

.favorite:focus-visible {
  outline: none;
  box-shadow: var(--focus-ring);
}

.favorite-on {
  color: var(--feedback-favorite);
}

.favorite .icon {
  width: 18px;
  height: 18px;
}

.body {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: 12px;
  padding: 20px 24px 24px;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 0;
  padding: 0;
  list-style: none;
}

/* Code/Tag: JetBrains Mono Medium 12/16 */
.tag {
  padding: 4px 8px;
  border-radius: var(--radius-tag);
  background-color: var(--neutral-100);
  font-family: var(--font-code);
  font-size: 12px;
  font-weight: 500;
  line-height: 16px;
  color: var(--neutral-600);
}

/* Heading/H3: Manrope Bold 20/28 */
.title {
  margin: 0;
  font-family: var(--font-heading);
  font-size: 20px;
  font-weight: 700;
  line-height: 28px;
  transition: color 0.2s;
}

/* Body/Small: Inter 14/20 */
.description {
  margin: 0;
  font-size: 14px;
  line-height: 20px;
  color: var(--neutral-600);
}

/* в свёрнутом виде — две строки */
.collapsed {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.footer {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 4px 12px;
  margin-top: auto;
  padding-top: 4px;
}

/* Label/Field: Inter Medium 14/20 */
.link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  min-height: 44px;
  padding: 0;
  border: none;
  background: none;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  line-height: 20px;
  color: var(--brand-primary);
  white-space: nowrap;
  cursor: pointer;
}

.link:hover {
  color: var(--brand-primary-hover);
}

.link .icon {
  width: 16px;
  height: 16px;
  transition: transform 0.2s;
}

.link .icon.rotated {
  transform: rotate(-90deg);
}

/* Caption: Inter 12/16 */
.meta {
  font-size: 12px;
  line-height: 16px;
  color: var(--neutral-400);
  white-space: nowrap;
}
</style>
