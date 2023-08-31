<template lang="pug">
.fill-height.home-page.pa-0.ma-0
  v-row.pa-0.ma-0.upper-row
    upper-title.ma-0(:icon="'heart'" :title-class="'dark-background'")
    w-search-bar.ma-0(@change="searchBy")
  .scroll.scrollbar-hide.ma-0.justify-top.align-center(:style="scrollSize")
    category-list.pb-4.pt-6(:items="categories" :title="'Categories'")
    //- ads-carousel.pt-4
    //- shops-list.pb-4
    //- item-list.pb-4
    shop-vertical-list.px-4.pb-4(:items="itemsvertical" :title="'Products'")
  v-row.pt-8.pb-6.lower-tile(dense :class="titleClass")
    v-col.text-center(:cols="2")
    v-col.text-center(:cols="8")
    v-col.text-center(:cols="2")
      v-btn(icon color="white" style='background-color: orange; position: fixed; bottom: 120px; right: 20px;' v-on:click="redirectToPage")
        v-icon {{ plusIcon }}

//- // </template>

<script>
import { mapGetters } from 'vuex'
import { mdiPlus } from '@mdi/js'

// import UpperTitle from '../components/UpperTitle.vue'
import WSearchBar from '../components/componenets-custom/WSearchBar.vue'
// import AdsCarousel from '../components/home/AdsCarousel.vue'
// import ShopsList from '../components/home/ShopsList.vue'
// import ItemList from '../components/home/ItemList.vue'
import CategoryList from '../components/home/CategoryList.vue'
import ShopVerticalList from '../components/home/ShopVerticalList.vue'

export default {
  name: 'HomePage',
  components: {
    // UpperTitle,
    WSearchBar,
    // AdsCarousel,
    // ShopsList,
    // ItemList,
    CategoryList,
    ShopVerticalList
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
