<!-- eslint-disable prettier/prettier -->
<template>
  <div class="view-ui">
    <template v-if="editMode">
      <h4 class="view-ui-header">
        Edit Task
      </h4>
      <label>Task title: </label>
      <InputUI v-model="editTitle" label="Edit task" placeholder="Edit task" size="m" variant="main" required
        style="max-width: 220px; margin-bottom: 10px" />
      <label style="margin-left: 10px">Description: </label>
      <InputUI v-model="editDescription" label="Edit description" placeholder="Edit description" size="m" variant="main"
        style="max-width: 220px" />
      <div style="margin-top: 18px; display: flex; gap: 10px">
        <ButtonUI label="Save" class="main-var" size="m" @click="saveEdit" />
        <ButtonUI label="Cancel" class="main-var" size="m" @click="$emit('cancel-edit')" />
      </div>
    </template>
    <template v-else>
      <h4 class="view-ui-header">
        {{ uTask }}
      </h4>
      <p>{{ taskDis }}</p>
      <slot></slot>
      <div style="margin-top: 18px; display: flex; gap: 10px">
        // eslint-disable-next-line vue/max-attributes-per-line
        <ButtonUI label="Edit" class="main-var" size="m" @click="$emit('edit')" />
        <ButtonUI label="Close" class="main-var" size="m" @click="$emit('close')" />
      </div>
    </template>
  </div>
</template>

<script>
import ButtonUI from './ButtonUI.vue'
import InputUI from './InputUI.vue'

export default {
  name: 'ViewUI',
  components: {
    ButtonUI,
    InputUI
  },
  props: {
    uTask: String,
    taskDis: String,
    editMode: {
      type: Boolean,
      default: false
    },
    editTitle: String,
    editDescription: String
  },
  methods: {
    saveEdit() {
      this.$emit('save-edit', { title: this.editTitle, description: this.editDescription })
    }
  }
}
</script>

<style>
.view-ui {
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 40%;
  transition: border-color 0.3s, box-shadow 0.3s;
  outline: none;
  background: #fff;
  padding: 18px 12px;
}

.view-ui-header {
  background-color: #c5eefc;
  color: #333;
  margin-bottom: 12px;
  padding: 6px 0;
}
</style>
