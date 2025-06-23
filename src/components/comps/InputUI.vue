<template>
  <input
    :title="label || placeholder"
    :aria-label="label || placeholder"
    :placeholder="placeholder"
    :type="type"
    :value="modelValue"
    :class="['input-ui', variantClass, sizeClass, { 'input-error': isError }]"
    @blur="validateInput"
    @keyup.enter="handleSubmit"
    @input="$emit('update:modelValue', $event.target.value)"
  >
</template>

<script>
export default {
  name: 'InputUI',
  props: {
    modelValue: {  
      type: String,
      default: ''
    },
    label: {      
      type: String,
      default: ''
    },
    placeholder: { 
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
      default: 'main',
      validator: value => ['main', 'second'].includes(value)
    },
    type: {
      type: String,
      default: 'text',
      validator: value => ['text', 'password', 'email', 'number', 'search'].includes(value)
    },
    required: {    
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isError: false
    };
  },
  computed: {
    variantClass() {
      return `input-ui-${this.variant}-var`;
    },
    sizeClass() {
      return `input-size-${this.size}`;
    }
  },
  methods: {
    handleSubmit() {
      this.validateInput();
      if (!this.isError) {
        this.$emit('submit', this.modelValue);
      }
    },
    validateInput() {
      this.isError = this.required && this.modelValue.trim() === '';
    }
  }
};
</script>

<style scoped>
.input-ui {
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  transition: border-color 0.3s, box-shadow 0.3s;
  outline: none;
}

.input-ui:focus {
  border-color: #4d90fe;
  box-shadow: 0 0 5px rgba(77, 144, 254, 0.5);
}

.input-ui-main-var {
  background-color: #c5eefc;
  color: #333;
}

.input-ui-second-var {
  background-color: #e0e0e0;
  color: #666;
}

.input-ui-main-var:hover {
  background-color: #b0e0f0;
}

.input-ui-second-var:hover {
  background-color: #d0d0d0;
}

.input-error {
  border-color: #e74c3c !important;
  background-color: #ffebee;
}

.input-size-s {
  font-size: 12px;
  padding: 6px 8px;
  height: 28px;
}

.input-size-m {
  font-size: 14px;
  padding: 8px 12px;
  height: 32px;
}

.input-size-l {
  font-size: 16px;
  padding: 10px 14px;
  height: 36px;
}

.input-size-xl {
  font-size: 18px;
  padding: 12px 16px;
  height: 40px;
}
</style>