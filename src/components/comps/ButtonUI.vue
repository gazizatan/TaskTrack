
<template>
  <button 
    :title="label"
    :aria-label="label"
    @click="handleClick"
    :disabled="loading"
    :class="['button-ui', variantClass, { 'button-loading': loading }, sizeClass]"
    :style="{ fontSize: buttonSize }"
  >
    <div v-if="loading" class="loader"></div>
    <template v-else>
      <slot name="icon"></slot>
      <slot>{{ label }}</slot>
    </template>
<!-- slot написать , в котором можно иконки добавить, тайп баттон, сайз как в инпуте, loader проверить-->
  </button>
</template>

<script>
/* eslint-disable */
export default {
  name: 'ButtonUI',
  props: {
    label: {
      type: String,
      default: ''
    },
    size: {
      type: String,
      default: 'm',
      validator: value => ['s', 'm', 'l', 'xl'].includes(value)
    },
    variant: {
      type: String,
      default: 'main-var' 
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isLoading: this.loading
    }
  },
  watch: {
    loading(newValue) {
      this.isLoading = newValue
    }
  },
  computed: {
    variantClass() {
      return `${this.variant}-var`;
    },
    sizeClass() {
      return `input-size-${this.size}`;
    }
  },

  methods: {
    handleClick() {
      if (!this.loading) {
        this.$emit('сlick'); 
      }
    }
  },
};
</script>

<style scoped>
.loader {
  border: 5px solid #f3f3f3;
  border-radius: 50%;
  border-top: 5px solid #3498db;
  width: 20px;
  height: 20px;
  animation: spin 2s linear infinite;
}
.button-ui {
  font-family: 'Arial', sans-serif;
  border-radius: 10px;
  cursor: pointer;
  padding: 8px 16px;
  transition: background-color 0.2s ease;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.button-ui:disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

.main-var {
  background-color: #baebff;
  border: 2px solid #a0d4f5;
}
.main-var:hover {
  background-color: #a0d4f5;
}
.main-var:active {
  background-color: #8cc3e6;
}

.second-var {
  background-color: #e0e0e0;
  border: 2px solid #c0c0c0;
}
.second-var:hover {
  background-color: #d5d5d5;
}
.second-var:active {
  background-color: #b0b0b0;
}

.loader {
  border: 3px solid rgba(255, 255, 255, 0.3);
  border-top: 3px solid #ffffff;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  animation: spin 0.8s linear infinite;
  box-sizing: border-box;
}

.input-size-s {
  font-size: 12px;
  padding: 6px;
}
.input-size-m {
  font-size: 14px;
  padding: 8px;
}
.input-size-l {
  font-size: 16px;
  padding: 10px;
}
.input-size-xl {
  font-size: 18px;
  padding: 12px;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
