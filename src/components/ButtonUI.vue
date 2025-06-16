<template>
  <button 
    :title="label"
    :aria-label="label"
    @click="handleClick"
    :disabled="loading"
    :class="['button-ui', variantClass, { 'button-loading': loading }]"
    :style="{ fontSize: buttonSize }"
  >
    <span v-if="loading" class="spinner"></span>
    <span v-else-if="buttonText">{{ buttonText }}</span>
    <span v-else>{{ label }}</span>
  </button>
</template>

<script>
export default {
  name: 'ButtonUI',
  props: {
    label: {
      type: String,
      default: ''
    },
    size: {
      type: String,
      default: '20px'
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
      buttonText: '',
      buttonSize: '',
    };
  },
  computed: {
    variantClass() {
      return `${this.variant}-var`;
    }
  },
  mounted() {
    this.buttonText = this.label;
    this.buttonSize = this.size;
  },

  methods: {
    handleClick() {
      if (!this.loading) {
        this.$emit('klick'); 
      }
    }
  },
  watch: {
    label(newLabel) {
      this.buttonText = newLabel;
    },
    size(newSize) {
      this.buttonSize = newSize;
    }
  }
};
</script>

<style scoped>
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

.spinner {
  border: 3px solid rgba(255, 255, 255, 0.3);
  border-top: 3px solid #ffffff;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  animation: spin 0.8s linear infinite;
  box-sizing: border-box;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
