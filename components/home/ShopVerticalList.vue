<template lang="pug">
.shop-list
  v-row.pa-0.ma-0.px-4
    v-col.pa-0.ma-0
      p.text-h6.font-weight-medium.pt-4.mb-2(v-if="title") {{ title }}
    v-col.pa-0.ma-0.d-flex.mb-2
      v-row.pt-4.mb-1.pr-2.align-end.justify-end(@click="")
        p.mb-0 View all
          w-icon.ml-3(
            :height="20"
            :width="20"
            :icon-name="'arrow-forward-outline'"
            :icon-fill="this.$vuetify.theme.themes.light.brown"
            @click=""
          )

  v-item-group.pt-2.pb-8
    template(v-for='item in items')
      v-row.d-flex.flex-column.px-4.pb-4.justify-center
        v-card.rounded-lg(
            @click=""
            outlined
            height="135"
          )
          v-row
            v-col.pr-0(:cols="4")
              v-img.rounded-lg.ma-2(:src="require(`../../assets/food/${item.src.toLowerCase()}.jpg`)" width="100" height="100")
                v-chip.ma-1.rounded-xl(outlined :color="$vuetify.theme.themes.light.primary") {{item.discount * 100}}%
            v-col.py-2.d-flex.flex-column.px-2(:cols="5")
              p.secondary--text.font-weight-medium.mb-0.pt-4 {{item.name}}
              p.caption.darkGrey--text.font-weight-light.mb-0 {{item.quantity}}
              v-row.mb-3.py-2.px-0.pl-3
                  p.caption.darkGrey--text.font-weight-light.mb-0  Stock: {{item.stockLeft}}

              p.caption.darkGrey--text.font-weight-light Expired Date: {{item.expiredDate}}
                  //- v-chip.mt-auto.mr-2.rounded-xl(outlined :color="$vuetify.theme.themes.light.primary") {{item.stockLeft}}
            v-col.mb-3.pl-0.py-2(:cols="3")
              p.primary--text.font-weight-medium.pt-4.pr-1 {{$formatCurrency(item.originalPrice)}}
                //- p.primary--text.font-weight-medium.pt-4.pr-1 {{$formatCurrency(item.originalPrice)}}
                del dd
          //- v-row
          //-   v-col(:cols="5")
          //-     v-img.rounded-lg.ma-3(:src="require(`../../assets/food/${item.src.toLowerCase()}.jpg`)" width="110" height="110")
          //-       v-chip.ma-1.rounded-xl(outlined :color="$vuetify.theme.themes.light.primary") {{item.discount * 100}}%
          //-   //- v-col.py-2.pl-0.d-flex.flex-column(:cols="4")
          //-   v-col.py-2.pl-0.d-flex.flex-column(:cols="6")
          //-     v-row
          //-       p.secondary--text.font-weight-medium.mb-0.pt-4 {{$strLimit(item.name, 16)}}
          //-         p.secondary--text.font-weight-medium.pt-4 {{item.originalPrice}}
          //-     p.caption.darkGrey--text.font-weight-light.mb-0 {{item.quantity}}
          //-     p.caption.darkGrey--text.font-weight-light.mb-0 Stock: {{item.stockLeft}}
          //-     p.caption.darkGrey--text.font-weight-light.mb-0
          //-       del dd

          //-     p.caption.darkGrey--text.font-weight-light.mb-0 Expired Date: {{item.expiredDate}}

</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import WCard from '../componenets-custom/WCard.vue'

export default {
  name: 'ShopVerticalList',
  components: {
    WCard
  },
  props: {
    title: {
      type: String,
      default: null
    },
    items: {
      type: Array || Object,
      default: null
    },
    tag: {
      type: String,
      default: null
    }
  },
  data () {
    return {
    }
  },
  computed: {
    ...mapGetters({
      widthX: 'screen/getWidthClass',
      // foods: 'home/getRecommendedFoods',
      shops: 'home/getShops',
      categories: 'home/getCategories',
      itemsvertical: 'home/getVerticalFoods'
    })
  },
  methods: {
  //   ...mapActions({
  //     changeSelectedJob: 'home/changeSelectedJob'
  //   }),
  //   getCompany (id) {
  //     return this.companies.find((company) => {
  //       return company.id === id
  //     })
  //   },
  //   getTag (cid) {
  //     for (let i = 1; i <= this.categories.length; i++) {
  //       if (cid.includes(i)) {
  //         return this.categories[i - 1].name
  //       }
  //     }
  //   },
  //   goToJobDetailsPage (item) {
  //     this.changeSelectedJob(item)
  //     this.$router.push('/jobdetails')
  //   }
  // }
    ...mapActions({
      changeSelectedCategory: 'home/changeSelectedCategory'
    }),
    getShopName (id) {
      return this.shops.find((shop) => {
        return shop.id === id
      }).name
    },
    // job
    cardColor (id) {
      if (id % 3 === 1) {
        return '#404348'
      } else if (id % 3 === 2) {
        return '#918679'
      } else {
        return '#FEB81E'
      }
    },
    getName (name) {
      return name === 'IT' ? 'information technology' : name.toLowerCase()
    },
    viewAllCategories () {
      this.$router.push('/categories')
    },
    goToJobsPage (item) {
      this.changeSelectedCategory(item)
      this.$router.push('/jobs')
    }
  }
}
</script>

<style lang="scss" scoped>
.min-350-width {
  min-width: 353px !important;
}

.v-chip.v-chip--outlined.v-chip.v-chip {
  background-color: white !important;
  border-width: 2px;
  font-weight: 400;
  font-size: 13px;
  height: 20px;
  max-width: 100px;
}
</style>
