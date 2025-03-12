<template>
  <div class="select-unselect-container">
    <div class="options-section">
      <div class="column">
        <h3>Available Options</h3>
        <div class="options-list">
          <div
            v-for="option in availableOptions"
            :key="option.id"
            class="option"
            @click="moveOption(option, 'available', 'disabled')"
          >
            {{ option.label }}
          </div>
        </div>
      </div>

      <div class="column">
        <h3>Disabled options</h3>
        <div class="options-list">
          <div
            v-for="option in disabledOptions"
            :key="option.id"
            class="option"
            @click="moveOption(option, 'disabled', 'available')"
          >
            {{ option.label }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import fieldMixin from '../FieldMixin';

interface Option {
  id: string;
  label: string;
}

const props = defineProps({
  field: {
    type: Object,
    required: true,
  },
  modelValue: {
    type: Array as () => string[],
    default: () => [],
  },
});

const emit = defineEmits(['update']);
const { handleChange } = fieldMixin.setup(props, { emit });

const availableOptions = ref<Option[]>([]);
const disabledOptions = ref<Option[]>([]);

const initializeOptions = () => {
  if (!props.field.options) return;
  
  const currentDisabled = props.modelValue || [];
  availableOptions.value = props.field.options.filter(
    opt => !currentDisabled.includes(opt.id)
  );
  disabledOptions.value = props.field.options.filter(
    opt => currentDisabled.includes(opt.id)
  );
};

const moveOption = (option: Option, from: 'available' | 'disabled', to: 'available' | 'disabled') => {
  const sourceList = from === 'available' ? availableOptions : disabledOptions;
  const targetList = to === 'available' ? availableOptions : disabledOptions;

  const index = sourceList.value.findIndex(opt => opt.id === option.id);
  if (index !== -1) {
    const [movedOption] = sourceList.value.splice(index, 1);
    targetList.value.push(movedOption);
    
    handleChange({
      target: {
        value: disabledOptions.value.map(opt => opt.id),
      },
    });
  }
};

watch(() => props.field.options, initializeOptions, { immediate: true });
watch(() => props.modelValue, initializeOptions);
</script>

<style scoped>
.select-unselect-container {
  width: 100%;
}

.options-section {
  display: flex;
  gap: 2rem;
  justify-content: space-between;
}

.column {
  flex: 1;
}

h3 {
  margin-bottom: 0.5rem;
  font-weight: normal;
  color: #fff;
}

.options-list {
  border: 1px solid #333;
  min-height: 150px;
  padding: 0.5rem;
  background: #000;
}

.option {
  text-align: left;
  cursor: pointer;
  color: #fff;
}

.option:hover {
  background-color: #333;
}

@media (max-width: 768px) {
  .options-section {
    flex-direction: column;
    gap: 1rem;
  }
}
</style> 