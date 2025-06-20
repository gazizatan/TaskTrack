<template>
  <!-- <input
    :title="value"
    :aria-label="value"
    type="" 
    v-model="value"
    :value = "value"
    :class="['input-ui', variantClass, sizeClass, { 'input-error': isError }]"
    @blur="validateInput"
    @keyup.enter="handleSubmit"
    
  > -->
  <input
    :title="value"
    :aria-label="value"
    :placeholder="value"
    type="" 
    :value = "value"
    :class="['input-ui', variantClass, sizeClass, { 'input-error': isError }]"
    @blur="validateInput"
    @keyup.enter="handleSubmit"
    @input = "$emit('update:value',$event.target.value)"
  >
  <!-- тайп универсальный, placeholder, :value vs v-model, how it works, @input-->
</template>

<script>
/* eslint-disable */
export default {
  name: 'InputUI',
  props: {
    value: {
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
      default: 'main'
    },
    type:{
      type: String,
      default: 'text'
    }
  },
  data() {
    return {
      isError: false
    };
  },
  computed: {
    variantClass() {
      return `input-ui-${this.variant}-varik`
    },
    sizeClass() {
      return `input-size-${this.size}`;
    }
  },
  methods: {
    handleSubmit() {
      this.validateInput();
      if (!this.isError) {
        this.$emit('submit', this.value);
      }
    },
    validateInput() {
      this.isError = this.value.trim() === '';
    }
  },
};
</script>

<style>
.input-ui {
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 90%;
  transition: border-color 0.3s, background-color 0.3s;
}

.input-ui-main-varik {
  background-color: #c5eefc;
  color: #333;
}
.input-ui-second-varik {
  background-color: #e0e0e0;
  color: #666;
}
.input-ui-main-varik:hover {
  background-color: #d0d0d0;
}
.input-ui-second-varik:hover {
  background-color: #c0c0c0;
}

.input-error {
  border-color: red !important;
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
</style>
