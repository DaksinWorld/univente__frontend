<template>
  <div>
    <input class="form-control my-2" type="search" v-model="searchValue">
    <table class="table b-1" v-if="sortedData">
      <thead class="thead-dark">
      <tr>
        <th @click="pickedBy('img')" :class="{active: picked === 'img'}">Image</th>
        <th @click="pickedBy('name')" :class="{active: picked === 'name'}">First Name</th>
        <th @click="pickedBy('description')" :class="{active: picked === 'description'}">Second Name</th>
        <th @click="pickedBy('location')" :class="{active: picked === 'location'}">Description</th>
        <th @click="pickedBy('language')" :class="{active: picked === 'language'}">Subtitle</th>
        <th>Edit</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(d, i) in sortedData" :key="i">
        <img v-if="d.images[0]" width="80" height="80" :src="`${imageUrl}${d.images[0].url}`" alt="">
        <td v-if="d.firstName">
          <h4>{{d.firstName}}</h4>
        </td>
        <td v-if="d.secondName">
          <h4>{{d.secondName}}</h4>
        </td>
        <td v-if="d.description">
          <h4>{{d.description}}</h4>
        </td>
        <td v-if="d.subtitle">
          <h4>{{d.subtitle}}</h4>
        </td>
        <td>
          <button class="btn btn-primary">
            <nuxt-link class="text-white" :to="`/admin/worker/${d._id}`">
              Edit
            </nuxt-link>
          </button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import {imageUrl} from "../assets/data";

export default {
  data: () => {
    return {
      jwtToken: '',
      data: [],
      picked: '',
      searchValue: '',
      inputVal: '',
      imageUrl: imageUrl
    }
  },
  async mounted(){
    try {
      if (this.jwtToken){
        await this.$store.dispatch('requests/checkJwt')
      }
      this.data = await this.$store.dispatch('requests/getAll', 'workers')
    } catch (e) {
      this.$store.dispatch('setMessage', {
        value: e.message,
        type: 'warning'
      })
      localStorage.removeItem('jwt')
    }
  },
  methods: {
    pickedBy(text){
      this.picked = text
    }
  },
  computed: {
    sortedData() {
      return [...this.data]
        .sort((a,b) => {
          return a[this.picked] > b[this.picked] ? 1 : -1
        })
        .filter((e) => {
          if(this.searchValue && e.description){
            return e.description.includes(this.searchValue)
          }
          return e
        })
    }
  }
}
</script>

<style scoped>

</style>
