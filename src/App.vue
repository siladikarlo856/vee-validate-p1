<template>
  <EditorComponent
    v-model:name="name"
    v-model:description="myObject.description"
  />
</template>

<script>
import { ref, reactive } from 'vue';
import EditorComponent from './components/EditorComponent.vue';

export default {
  name: 'App',
  components: { EditorComponent },
  setup() {
    const myObject = reactive({ name: 'Name123456789', description: 'Desc' });

    const name = ref('');

    setTimeout(() => {
      console.log('timeout');
      name.value = "Test"
    }, 1000);
    return {
      myObject,
      name,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
