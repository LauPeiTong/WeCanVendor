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
            :color="color"
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
              p.primary--text.font-weight-medium.pt-4.pr-1  {{$formatCurrency(item.originalPrice - (item.discount * 100))}}
                //- p.primary--text.font-weight-medium.pt-4.pr-1 {{$formatCurrency(item.originalPrice)}}
              v-row.pl-3
                del.caption.darkGrey--text {{$formatCurrency(item.originalPrice)}}
              v-row.py-0.pl-3
                p.secondary--text.font-weight-medium.pt-8.mb-0(:color="cornerIconColor") {{item.daysLeft}}

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
    //- v-row.pt-1.pb-2.lower-tile(dense :class="titleClass")
    //-   v-col.text-center(:cols="2")
    //-   v-col.text-center(:cols="8")
    //-   v-col.text-center(:cols="2")
    //-     v-btn(icon color="orange")
    //-       v-icon {{ plusIcon }}

    //- v-row.mb-2.ma-0(no-gutters)
    //-   v-btn(icon color="orange")
    //-     v-icon {{ plusIcon }}
    //- v-btn(size="small" rounded)
    //-   eva-icon.px-0.pt-0(name="plus-outline" :fill="$vuetify.theme.themes.light.primary" height="50" width="50")

    //- v-btn(icon="mdi-plus-outline", size="small")
        eva-icon(name="award" :fill="$vuetify.theme.themes.light.primary" height="50" width="50")

</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import { mdiPlus } from '@mdi/js'
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
    },
    cornerIcon: {
      type: String,
      default: null
    },
    color: {
      type: String,
      default: null
    }
  },
  data () {
    return {
      cornerIconColor: '#FEB81E',
      plusIcon: mdiPlus
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

.v-btn.v-btn--icon.v-size--large {
  padding: 0px !important;
}
.fixed-button {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 9999; /* Adjust the z-index value as needed */
  }

.lower-title {
  position: absolute;
  top: 0;
  width: 100%;
  border-radius: 0px 0px 25px 25px;
  z-index: 100;
}
</style>
