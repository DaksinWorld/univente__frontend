<template>
  <div>
    <input class="form-control my-2" type="search" v-model="searchValue">
    <table class="table b-1" v-if="sortedData">
      <thead class="thead-dark">
      <tr>
        <th @click="pickedBy('image')" :class="{active: picked === 'image'}">Image</th>
        <th @click="pickedBy('name')" :class="{active: picked === 'name'}">Name</th>
        <th @click="pickedBy('description')" :class="{active: picked === 'description'}">Location</th>
        <th @click="pickedBy('location')" :class="{active: picked === 'location'}">Description</th>
        <th @click="pickedBy('language')" :class="{active: picked === 'language'}">Total Students</th>
        <th>Edit</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(d, i) in sortedData" :key="i">
        <td v-if="d.images[1].url">
          <img height="50" width="50" :src='imageUrl + d.images[1].url' alt="image">
        </td>
        <td v-if="d.nameEn">
          <h4>{{d.nameEn}}</h4>
        </td>
        <td v-if="d.location">
          <h4>{{d.location}}</h4>
        </td>
        <td v-if="d.descriptionEn">
          <h4>{{d.descriptionEn}}</h4>
        </td>
        <td v-if="d.totalStudents">
          <h4>{{d.totalStudents}}</h4>
        </td>
        <td>
          <button class="btn btn-primary">
            <nuxt-link class="text-white" :to="`/admin/univ/${d._id}`">
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
      this.data = await this.$store.dispatch('requests/getAll', 'univ')
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
          if(this.searchValue && e.name){
            return e.name.includes(this.searchValue)
          }
          return e
        })
    }
  }
}
</script>

<style scoped>

</style>
