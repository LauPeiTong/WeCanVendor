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
              v-col(:cols="5")
                v-img.rounded-lg.ma-3(:src="require(`../../assets/home/shops/${item.src.toLowerCase()}.jpg`)" width="110" height="110")
              v-col.py-2.pl-0.d-flex.flex-column(:cols="7")
                p.secondary--text.font-weight-medium.mb-0.pt-4 {{item.name}}
                p.caption.darkGrey--text.font-weight-light.mb-0
                  //- del is for strikethrough
                  del dd
                p.caption.darkGrey--text.font-weight-light.mb-0 {{item.address}}
                p.caption.darkGrey--text.font-weight-light.mb-0.mt-0 {{item.distance}} km .
                  |
                  img.ml-1(width="14" height="14" :src="require(`../../assets/home/star.jpg`)")
                  |   {{item.rate}}
                p.caption.darkGrey--text.font-weight-light.mb-0.mt-0 {{item.duration}} mins .
                  |
                  img.mx-1(width="13" height="12" :src="require(`../../assets/home/motorcycle.jpg`)")
                  |   {{$formatCurrency(item.deliveryFee)}}
                v-row.d-flex.flex-row.mb-4.pl-2
                  template(v-for='tag in item.tags')
                    v-chip.mt-auto.mr-2.rounded-xl(outlined :color="$vuetify.theme.themes.light.primary") {{tag}}

</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import WCard from '../componenets-custom/WCard.vue'

export default {
  name: 'ItemVerticalList',
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
      shops: 'home/getShops',
      categories: 'home/getCategories'
    })
  },
  methods: {
    ...mapActions({
      changeSelectedJob: 'home/changeSelectedJob'
    }),
    getCompany (id) {
      return this.companies.find((company) => {
        return company.id === id
      })
    },
    getTag (cid) {
      for (let i = 1; i <= this.categories.length; i++) {
        if (cid.includes(i)) {
          return this.categories[i - 1].name
        }
      }
    },
    goToJobDetailsPage (item) {
      this.changeSelectedJob(item)
      this.$router.push('/jobdetails')
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
