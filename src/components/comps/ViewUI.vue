<template>
    <div class="view-ui" >
        <H4 class="view-ui-header">
            {{ utask }}
        </H4>
        <p> {{ taskDis }}</p>
        <ButtonUI label = "Delete" class="main-var" size="m" />
        <ButtonUI label = "Edit" class="main-var" size="m" />
        <ButtonUI label = "Close" class="main-var" size="m" />
    </div>
</template>

<script>
import ButtonUI from './ButtonUI.vue';

export default {
  name: 'ViewUI',
  props: {
    utask: String,
    taskDis: String
  },
  components: {
      ButtonUI,
  }
}
</script>

<style>
.view-ui{
    border: 1px solid #ccc;
    border-radius: 4px;
    width: 100%;
    transition: border-color 0.3s, box-shadow 0.3s;
    outline: none;
}
.view-ui-header{
    background-color: #c5eefc;
    color: #333;
}

</style>