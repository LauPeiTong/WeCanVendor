<template lang="pug">
.fill-height.tracker-page.pa-0.ma-0
  v-row.pa-0.ma-0.upper-row
    upper-title.ma-0(:title="'Products'" :icon="'more-vertical'" :title-class="'dark-background'")
    w-search-bar.ma-0(@change="searchBy")
  .scroll.ma-0.justify-top.align-center(:style="scrollSize")
    food-list.pt-10.pb-2
    v-row.pt-8.pb-6(dense)
      v-col.text-center(:cols="2")
      v-col.text-center(:cols="8")
      v-col.text-center(:cols="2")
        v-btn.add-button(icon color="white" v-on:click="goToAddProductPage")
          v-icon {{ plusIcon }}
</template>

<script>
import { mapGetters } from 'vuex'
import { mdiPlus } from '@mdi/js'

import UpperTitle from '../../components/UpperTitle.vue'
import WSearchBar from '../../components/componenets-custom/WSearchBar.vue'
import FoodList from '../../components/products/FoodList.vue'

export default {
  name: 'ProductsPage',
  components: {
    UpperTitle,
    WSearchBar,
    FoodList
  },
  layout: 'default',
  data () {
    return {
      search: null,
      plusIcon: mdiPlus
    }
  },
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollClass',
      shops: 'home/getShops'
    })
  },
  methods: {
    searchBy (newValue) {
      this.search = newValue
    },
    goToAddProductPage () {
      this.$router.push('/products/new')
      // const response = await this.$axios.get('/api/donations/?total=true')
      // console.log(response)
    }
  }
}
</script>

<style lang="scss" scoped>
:deep(.scroll) {
  overflow-x: hidden;
  overflow-y: auto;
  width: 100% !important;
}
.lower-title {
  position: absolute;
  top: 0;
  width: 100%;
  border-radius: 0px 0px 25px 25px;
  z-index: 100;
}
.add-button {
  background-color: #FAAF08;
  position: fixed;
  bottom: 120px;
  right: 20px;
}
</style>
