<template lang="pug">
.fill-height.tracker-page.pa-0.ma-0
  v-row.pa-0.ma-0.upper-row
    upper-title.ma-0(:title="'Food Tracker'" :icon="'more-vertical'")
    w-search-bar.ma-0(@change="searchBy")
  .scroll.ma-0.justify-top.align-center(:style="scrollSize")
    food-list.pt-10.pb-2
  v-row.pt-8.pb-6.lower-tile(dense)
    v-col.text-center(:cols="2")
    v-col.text-center(:cols="8")
    v-col.text-center(:cols="2")
      v-btn(icon color="white" style='background-color: orange; position: fixed; bottom: 120px; right: 20px;' v-on:click="redirectToPage")
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
      shops: 'home/getShops',
      itemsvertical: 'home/getVerticalFoods'
    //   recommendedJob: 'home/getRecommededJob'
    })
  },
  methods: {
    searchBy (newValue) {
      this.search = newValue
    },
    redirectToPage () {
      this.$router.push('/addproduct')
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
</style>
