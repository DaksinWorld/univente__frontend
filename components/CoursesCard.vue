<template>
  <div>
    <article class="card" v-if="data">
      <div class="card-top">
        <div class="card-header">
          <div class="logo"><img height="90" width="90" :src="`${imageUrl}${data.images[1].url}`" alt=""></div>
          <div class="card-university-info">
            <div class="d-flex flex-column">
              <span class="university-description" v-if="isIncludesSp">{{data.universitiesSp}}</span>
              <span class="university-description" v-else>{{data.universitiesEn}}</span>
              <span class="university-location d-flex align-items-center"><img width="20" height="20" src="https://gt.education/img/svg/home.svg" alt="location"> {{ data.location}}</span>
            </div>
            <span class="university-degree" v-if="isIncludesSp">Foundation Course/{{data.fieldOfCourseSp}}</span>
            <span class="university-degree" v-else>Foundation Course/{{data.fieldOfCourseEn}}</span>
          </div>
        </div>
        <div class="card-specialty">
          {{data.programs}}
        </div>
      </div>
      <div class="card-bottom">
        <ul class="card-detail-list">
          <li>
            Tuition fee
            <span>{{data.price}}</span>
          </li>
          <li>
            Application deadline
            <span>{{data.deadline}}</span>
          </li>
          <li>
            Start Date
            <span>{{data.startDate}}</span>
          </li>
        </ul>
        <div @click="addToCart" v-if="!isAddedToCart" class="btn btn-primary">
          Add to Cart
        </div>
        <nuxt-link v-else to="/cart?step=1">
          <div class="btn btn-primary col-lavender">
            Go To Cart
          </div>
        </nuxt-link>
      </div>
    </article>
  </div>
</template>

<script>
import { mapGetters} from "vuex";
import {imageUrl} from "../assets/data";

export default {
  props: {
    data: {
      type: Array,
      required: true
    }
  },
  data: () => {
    return {
      imageUrl: imageUrl
    }
  },
  computed: {
    ...mapGetters('cart', ['cartList']),
    isAddedToCart: function () {
      if (
        this.cartList.findIndex(
          item => item._id === this.data._id
        ) > -1
      )
        return true;
      return false;
    },
    isIncludesSp() {
      return this.$route.fullPath.includes('sp')
    }
  },
  methods: {
    addToCart() {
      let saledProduct = {
        ...this.data,
        qty: 1,
        type: 'course'
      }


      this.$store.dispatch('cart/addToCart', {product: saledProduct})
    }
  }
}
</script>

<style scoped>
@import "../assets/css/card.styles.css";
</style>
