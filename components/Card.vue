<template>
  <article class="card" v-if="data">
    <div class="card-top">
      <div class="card-header">
        <div class="logo">
          <div class="card-university-info">
            <div class="d-flex flex-column">
              <span class="university-description" v-if="isIncludesSp">{{ data.descriptionSp }}</span>
              <span class="university-description" v-else>{{ data.descriptionEn }}</span>
              <span class="university-location d-flex align-items-center"><img width="20" height="20"
                                                                               src="https://gt.education/img/svg/home.svg"
                                                                               alt="location"> {{ data.location }}</span>
            </div>
            <span class="university-degree" v-if="isIncludesSp">{{ data.degreeSp }}/{{ data.fieldOfStudySp }}</span>
            <span class="university-degree" v-else>{{ data.degreeEn }}/{{ data.fieldOfStudyEn }}</span>
          </div>
        </div>
        <div class="card-specialty">
          {{ data.programs }}
        </div>
      </div>
      <div class="card-bottom">
        <ul class="card-detail-list">
          <li>
            Tuition fee
            <span>{{ data.price }}</span>
          </li>
          <li>
            Duration
            <span>{{ (data.duration / 12).toFixed() }} Years</span>
          </li>
          <li>
            Language
            <span>{{ data.language }}</span>
          </li>
        </ul>
        <div @click="addToCart" v-if="!isAddedToCart" class="btn btn-primary">
          Add to Cart
        </div>
        <nuxt-link v-else-if="isIncludesSp" to="/sp/cart?step=1">
          <div class="btn btn-primary col-lavender">
            Go To Cart
          </div>
        </nuxt-link>
        <nuxt-link v-else to="/cart?step=1">
          <div class="btn btn-primary col-lavender">
            Go To Cart
          </div>
        </nuxt-link>
      </div>
    </div>
  </article>
</template>

<script>
import {imageUrl} from "../assets/data";
import {mapGetters} from "vuex";

export default {
  props: ['data'],
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
        type: 'program'
      }

      this.$store.dispatch('cart/addToCart', {product: saledProduct})
    }
  }
}
</script>

<style scoped>
@import "../assets/css/card.styles.css";
</style>
