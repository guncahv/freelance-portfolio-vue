<script>
import searchIcon from '../assets/icons/icon-search.svg'
import closeIcon from '../assets/icons/icon-close.svg'

/**
 * Поле поиска проектов (Input Field из ЛР №17).
 * Поддерживает v-model у родителя: принимает modelValue и сообщает об изменениях событием update:modelValue.
 */
export default {
  name: 'FilterInput',
  props: {
    modelValue: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      default: 'Поиск по проектам',
    },
    placeholder: {
      type: String,
      default: 'Название, описание или технология',
    },
  },
  emits: ['update:modelValue'],
  data() {
    return {
      query: this.modelValue,
      isFocused: false,
      searchIcon,
      closeIcon,
    }
  },
  watch: {
    // изменения в поле → родителю
    query(value) {
      this.$emit('update:modelValue', value)
    },
    // изменения у родителя (например, сброс) → в поле
    modelValue(value) {
      this.query = value
    },
  },
  methods: {
    clear() {
      this.query = ''
      this.$refs.input.focus()
    },
  },
}
</script>

<template>
  <div class="field">
    <label for="project-search" class="label">{{ label }}</label>
    <div class="control" :class="{ focused: isFocused }">
      <span class="icon" :style="{ '--icon': `url(${searchIcon})` }" aria-hidden="true"></span>
      <input
        id="project-search"
        ref="input"
        v-model.trim="query"
        type="search"
        class="input"
        :placeholder="placeholder"
        autocomplete="off"
        @focus="isFocused = true"
        @blur="isFocused = false"
      />
      <button v-if="query" type="button" class="clear" aria-label="Очистить поиск" @click="clear">
        <span class="icon" :style="{ '--icon': `url(${closeIcon})` }" aria-hidden="true"></span>
      </button>
    </div>
  </div>
</template>

<style scoped>
/* Input Field: вертикальный Auto Layout (подпись, поле), gap 6; поле высотой 48, радиус 12 */
.field {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 420px;
}

/* Label/Field: Inter Medium 14/20 */
.label {
  font-size: 14px;
  font-weight: 500;
  line-height: 20px;
  color: var(--neutral-900);
}

/* State: Default */
.control {
  display: flex;
  align-items: center;
  gap: 10px;
  min-height: 48px;
  padding: 0 8px 0 16px;
  border: 1.5px solid var(--neutral-300);
  border-radius: var(--radius-control);
  background-color: var(--neutral-0);
  color: var(--neutral-400);
  transition: border-color 0.15s, box-shadow 0.15s, color 0.15s;
}

/* State: Focused — обводка Brand/Primary и Focus/Ring */
.control.focused {
  border-color: var(--brand-primary);
  box-shadow: var(--focus-ring);
  color: var(--brand-primary);
}

/* Body/Medium: Inter 16/24 */
.input {
  flex: 1;
  min-width: 0;
  padding: 12px 0;
  border: none;
  outline: none;
  background: transparent;
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 24px;
  color: var(--neutral-900);
}

.input::placeholder {
  color: var(--neutral-400);
}

/* убираем системный крестик type="search" — используется свой */
.input::-webkit-search-cancel-button {
  display: none;
}

.clear {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border: none;
  border-radius: 8px;
  background: transparent;
  color: var(--neutral-600);
  cursor: pointer;
}

.clear:hover {
  background-color: var(--neutral-100);
  color: var(--neutral-900);
}

.clear .icon {
  width: 16px;
  height: 16px;
}
</style>
