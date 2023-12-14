<template>
  <div id="app">
    <DatePickerComponent />
  </div>
</template>

<script>
import DatePickerComponent from './components/DatePickerComponent.vue';

export default {
  components: {
    DatePickerComponent,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
