<template>
  <div class="header">
    <h1 class="title">Star Wars</h1>
    <p>Type Script, Services, Composition API, Rest API</p>
    <router-link to="/about">and more</router-link>
  </div>
  <div v-if="loading">
    <Spinner/>
  </div>
  <div v-else>
    <table>
      <thead>
        <tr>
          <td>name</td>
          <td>gender</td>
          <td>eye color</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(people, index) in peoples.results" :key="index">
          <td>{{ people.name }}</td>
          <td>{{ people.gender }}</td>
          <td>{{ people.eye_color }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import {defineComponent, onMounted, ref} from 'vue';
import Spinner from '@/components/UI/Spinner-ui.vue'
import DataService from '@/services/dataService';
import ResponseData from '@/types/ResponseData';
import Peoples from '@/types/Peoples';

export default defineComponent({
  components: {
    Spinner
  },
  setup() {

    const loading = ref(true as boolean)
    const peoples = ref({} as Peoples)

    onMounted(() => getPeople())

    const getPeople = () => 
      DataService.getAll()
      .then((res: ResponseData)=> {
        peoples.value = res.data
        loading.value = false
      })
      .catch((e: Error)=> console.log(e))

    return {
      loading,
      peoples
    }
  },  
})
</script>

<style>
.header {
  margin: 2rem 0;
  text-align: center;
}
</style>