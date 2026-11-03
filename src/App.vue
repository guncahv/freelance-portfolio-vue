<script>
import FilterInput from './components/FilterInput.vue'
import ProjectList from './components/ProjectList.vue'

import logoMark from './assets/icons/logo-mark.svg'
import homeIcon from './assets/icons/icon-home.svg'
import projectsIcon from './assets/icons/icon-projects.svg'
import aboutIcon from './assets/icons/icon-about.svg'
import contactIcon from './assets/icons/icon-contact.svg'
import arrowIcon from './assets/icons/icon-arrow-right.svg'
import photo from './assets/images/photo-specialist@1x.jpg'
import marketplaceCover from './assets/images/project-marketplace.svg'
import crmCover from './assets/images/project-crm.svg'
import fintechCover from './assets/images/project-fintech.svg'
import portfolioCover from './assets/images/project-portfolio.svg'

/**
 * Корневой компонент: хранит данные проектов и поисковый запрос,
 * фильтрует список вычисляемым свойством filteredProjects.
 */
export default {
  name: 'App',
  components: { FilterInput, ProjectList },
  data() {
    return {
      logoMark,
      photo,
      arrowIcon,
      navItems: [
        { id: 'home', label: 'Главная', icon: homeIcon },
        { id: 'projects', label: 'Проекты', icon: projectsIcon },
        { id: 'about', label: 'Обо мне', icon: aboutIcon },
        { id: 'contact', label: 'Контакты', icon: contactIcon },
      ],
      activeNav: 'home',
      // поисковый запрос — связан с FilterInput через v-model
      searchQuery: '',
      // флажок «Только избранные» — связан через v-model
      onlyFavorites: false,
      favoriteIds: [],
      projects: [
        {
          id: 1,
          title: 'Маркетплейс handmade-товаров',
          description: 'Каталог, корзина и кабинет мастера. Загрузка страниц стала быстрее на 40 %.',
          image: marketplaceCover,
          tags: ['React', 'TypeScript', 'Node.js'],
          year: 2026,
        },
        {
          id: 2,
          title: 'CRM для автосервиса',
          description: 'Запись клиентов, учёт запчастей и уведомления в Telegram для сети из 5 станций.',
          image: crmCover,
          tags: ['Vue', 'Laravel', 'MySQL'],
          year: 2026,
        },
        {
          id: 3,
          title: 'Лендинг финтех-стартапа',
          description: 'Адаптивный лендинг с калькулятором вклада. Конверсия в заявку выросла до 7,8 %.',
          image: fintechCover,
          tags: ['Next.js', 'Tailwind', 'Figma'],
          year: 2025,
        },
        {
          id: 4,
          title: 'Сайт-портфолио фотографа',
          description: 'Галерея работ с фильтрами по жанрам и онлайн-запись на фотосессию.',
          image: portfolioCover,
          tags: ['Vue', 'Vite', 'Firebase'],
          year: 2025,
        },
      ],
    }
  },
  computed: {
    normalizedQuery() {
      return this.searchQuery.trim().toLowerCase()
    },
    // фильтрация списка по поисковому запросу и флажку «Только избранные»
    filteredProjects() {
      return this.projects.filter((project) => {
        if (this.onlyFavorites && !this.favoriteIds.includes(project.id)) {
          return false
        }
        if (!this.normalizedQuery) {
          return true
        }
        const text = [project.title, project.description, ...project.tags].join(' ').toLowerCase()
        return text.includes(this.normalizedQuery)
      })
    },
  },
  methods: {
    toggleFavorite(id) {
      if (this.favoriteIds.includes(id)) {
        this.favoriteIds = this.favoriteIds.filter((favoriteId) => favoriteId !== id)
      } else {
        this.favoriteIds.push(id)
      }
    },
    resetFilter() {
      this.searchQuery = ''
      this.onlyFavorites = false
    },
    selectNav(id) {
      this.activeNav = id
    },
    scrollTo(id) {
      document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
    },
  },
}
</script>

<template>
  <header class="header">
    <a href="#home" class="logo" aria-label="aykhan.dev — на главную">
      <img :src="logoMark" alt="" width="36" height="36" />
      <span>aykhan<span class="logo-accent">.dev</span></span>
    </a>

    <nav aria-label="Основное меню">
      <ul class="nav">
        <li v-for="item in navItems" :key="item.id">
          <a
            :href="`#${item.id}`"
            class="nav-item"
            :class="{ active: item.id === activeNav }"
            :aria-current="item.id === activeNav ? 'page' : null"
            @click="selectNav(item.id)"
          >
            <span class="icon" :style="{ '--icon': `url(${item.icon})` }" aria-hidden="true"></span>
            {{ item.label }}
          </a>
        </li>
      </ul>
    </nav>

    <div class="actions">
      <span class="status">Открыт к проектам</span>
      <img :src="photo" alt="Фото специалиста" class="avatar" />
      <button type="button" class="btn btn-primary" @click="scrollTo('contact')">
        Связаться
        <span class="icon" :style="{ '--icon': `url(${arrowIcon})` }" aria-hidden="true"></span>
      </button>
    </div>
  </header>

  <main class="page">
    <section id="home" class="hero">
      <div class="hero-text">
        <span class="badge">Фриланс-разработчик · Vue · React · Node.js</span>
        <h1 class="h1">Создаю быстрые веб-приложения под ключ</h1>
        <p class="lead">
          Привет! Я Айхан. Помогаю стартапам и малому бизнесу запускать сайты и веб-сервисы: от прототипа в Figma до
          релиза.
        </p>
        <div class="hero-actions">
          <button type="button" class="btn btn-primary" @click="scrollTo('projects')">
            Смотреть проекты
            <span class="icon" :style="{ '--icon': `url(${arrowIcon})` }" aria-hidden="true"></span>
          </button>
          <button type="button" class="btn btn-secondary">Скачать резюме</button>
        </div>
      </div>
      <img :src="photo" alt="Айхан Рахманкулов, фриланс-разработчик" class="photo" />
    </section>

    <section id="projects" class="section">
      <h2 class="h2">Избранные проекты</h2>

      <form class="filters" role="search" @submit.prevent>
        <FilterInput v-model="searchQuery" />
        <label class="checkbox">
          <input v-model="onlyFavorites" type="checkbox" />
          Только избранные
        </label>
      </form>

      <ProjectList
        :projects="filteredProjects"
        :total="projects.length"
        :favorite-ids="favoriteIds"
        :query="searchQuery"
        @toggle-favorite="toggleFavorite"
        @reset-filter="resetFilter"
      />
    </section>

    <section id="contact" class="contact">
      <div>
        <h2 class="contact-title">Обсудим ваш проект?</h2>
        <p class="contact-text">Напишите на почту — отвечу в течение рабочего дня.</p>
      </div>
      <a class="btn btn-primary" href="mailto:hello@aykhan.dev">Написать</a>
    </section>
  </main>

  <footer class="footer">© 2026 aykhan.dev · Портфолио фриланс-разработчика</footer>
</template>

<style scoped>
/* Header Navigation из ЛР №17: высота 72, Space between, отступы 40 */
.header {
  position: sticky;
  top: 0;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  min-height: 72px;
  padding: 12px 40px;
  background-color: var(--neutral-0);
  box-shadow: var(--shadow-header);
}

.logo {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-family: var(--font-heading);
  font-size: 20px;
  font-weight: 800;
  line-height: 24px;
  color: var(--neutral-900);
  text-decoration: none;
}

.logo-accent {
  color: var(--brand-primary);
}

.nav {
  display: flex;
  gap: 4px;
  margin: 0;
  padding: 0;
  list-style: none;
}

.nav-item {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  min-height: 44px;
  padding: 12px 14px;
  border-radius: var(--radius-nav);
  font-size: 15px;
  font-weight: 500;
  line-height: 20px;
  color: var(--neutral-600);
  text-decoration: none;
  white-space: nowrap;
  transition: background-color 0.15s, color 0.15s;
}

.nav-item:hover {
  background-color: var(--neutral-100);
  color: var(--neutral-900);
}

.nav-item.active,
.nav-item.active:hover {
  background-color: var(--brand-primary-subtle);
  color: var(--brand-primary);
}

.nav-item .icon {
  width: 18px;
  height: 18px;
}

.actions {
  display: flex;
  align-items: center;
  gap: 16px;
}

.status {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 4px 10px;
  border: 1px solid var(--neutral-200);
  border-radius: 999px;
  font-size: 12px;
  line-height: 16px;
  color: var(--neutral-600);
  white-space: nowrap;
}

.status::before {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: var(--brand-accent);
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
}

.page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 40px 48px;
}

.hero {
  display: grid;
  grid-template-columns: 1fr minmax(260px, 340px);
  align-items: center;
  gap: 48px;
  padding: 64px 0 48px;
}

.hero-text {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 20px;
}

.badge {
  padding: 6px 12px;
  border-radius: 999px;
  background-color: var(--brand-primary-subtle);
  font-size: 12px;
  font-weight: 500;
  line-height: 16px;
  color: var(--brand-primary);
}

/* Heading/H1: Manrope ExtraBold 48/56 */
.h1 {
  margin: 0;
  max-width: 620px;
  font-family: var(--font-heading);
  font-size: 48px;
  font-weight: 800;
  line-height: 56px;
}

/* Body/Large: Inter 18/28 */
.lead {
  margin: 0;
  max-width: 560px;
  font-size: 18px;
  line-height: 28px;
  color: var(--neutral-600);
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.photo {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
  border-radius: var(--radius-card);
  box-shadow: var(--shadow-card-hover);
}

.section {
  padding: 32px 0;
}

/* Heading/H2: Manrope Bold 32/40 */
.h2 {
  margin: 0 0 20px;
  font-family: var(--font-heading);
  font-size: 32px;
  font-weight: 700;
  line-height: 40px;
}

.filters {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-end;
  gap: 16px 24px;
  margin-bottom: 20px;
}

.checkbox {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  min-height: 48px;
  font-size: 15px;
  font-weight: 500;
  color: var(--neutral-900);
  cursor: pointer;
}

.checkbox input {
  width: 18px;
  height: 18px;
  accent-color: var(--brand-primary);
}

.contact {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  margin-top: 32px;
  padding: 32px 40px;
  border: 1px solid var(--neutral-200);
  border-radius: var(--radius-card);
  background-color: var(--neutral-0);
  box-shadow: var(--shadow-card);
}

.contact-title {
  margin: 0 0 8px;
  font-family: var(--font-heading);
  font-size: 32px;
  font-weight: 700;
  line-height: 40px;
}

.contact-text {
  margin: 0;
  color: var(--neutral-600);
}

.footer {
  padding: 24px 20px 40px;
  text-align: center;
  font-size: 12px;
  line-height: 16px;
  color: var(--neutral-400);
}

@media (max-width: 1100px) {
  .header {
    flex-wrap: wrap;
    padding: 12px 20px;
  }

  .status {
    display: none;
  }
}

@media (max-width: 900px) {
  .page {
    padding: 0 20px 40px;
  }

  .hero {
    grid-template-columns: 1fr;
    padding-top: 40px;
  }

  .photo {
    max-width: 340px;
  }

  .h1 {
    font-size: 36px;
    line-height: 44px;
  }

  .contact {
    flex-direction: column;
    align-items: stretch;
    padding: 24px;
  }
}

@media (max-width: 720px) {
  .header {
    position: static;
  }

  nav {
    order: 3;
    width: 100%;
    overflow-x: auto;
  }

  .avatar {
    display: none;
  }
}
</style>
