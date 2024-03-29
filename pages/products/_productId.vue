<template lang="pug">
.fill-height.product-details-page.pa-0.ma-0.full-width(v-if="!!food")
  v-row.on-top.pt-8.pb-6.pa-0.ma-0.primary(dense)
    v-col.text-center(:cols="2")
      w-icon(
        :icon-name="'arrow-ios-back-outline'"
        :icon-fill="'white'"
        @click="goBackToProductsPage()"
      )
    v-col.text-center(:cols="8")
    v-col.text-center(:cols="2")
      w-icon(
        :icon-name="'heart'"
        :icon-fill="'white'"
        @click=""
      )

  .scroll.ma-0.justify-top.align-center.full-width(:style="scrollSize")
    v-img.absolute-position(height="220")
    v-row.pt-16
      v-col.pt-16.px-8.img-on-top.negative-margin.text-center(:cols="12")
        v-avatar(size="150" )
          v-img.white(:src="require(`../../assets/food/noitem.png`)" v-if="food.image_url == '' || food.image_url == null")
          v-img(:src="food.image_url" v-else)

      v-col.second-on-top.white(:cols="12")
        .food.px-4.pt-10
          .text-right
            v-chip.my-2.rounded-xl(outlined :color="$vuetify.theme.themes.light.primary") {{parseInt(food.discount)}}% discount
          //- Food
          v-card.py-2.px-3.rounded-lg(outlined)
            v-row
              v-col.pb-0(:cols="8")
                p.font-weight-medium.text-h6.secondary--text.mb-0 {{ food.name }}
                //- p.darkGrey--text Stock:
              v-col.pb-0.text-right
                p.font-weight-medium.text-h6.primary--text.mb-0 {{$formatCurrency(food.price)}}
                p.text-12.text-decoration-line-through.primary--text {{$formatCurrency(food.original_price)}}
            v-divider.my-2
            v-row.mb-2
              v-col.pb-0(:cols="6")
                p.font-weight-medium.secondary--text.mb-0 Expired date & time
              v-col.pb-0.text-right.pt-4
                p.font-weight-medium.success--text.mb-0 {{formatDate(new Date(food.expired_date))}}
            v-divider.my-2(v-if="food.status !== 'Expired'")
            v-row.mb-2(v-if="food.status !== 'Expired'")
              v-col.pb-0(:cols="6")
                p.font-weight-medium.secondary--text.mb-0 Time Left
              v-col.pb-0.text-right.pt-4
                p.font-weight-medium.success--text.mb-0(v-if="food.time_left.days > 0") {{food.time_left.days}} days
                p.font-weight-medium.primary--text.mb-0(v-else-if="food.time_left.hours > 0") {{food.time_left.hours}} hours
                p.font-weight-medium.danger--text.mb-0(v-else) Almost expired

          //- Details
          .py-2.rounded-lg(outlined)
            v-list
              v-list-group.bottom-gray(v-for="d in description"
                :key="d.title"
                v-model="d.active"
                no-action
              )
                template(v-slot:activator)
                  v-list-item-content
                    v-list-item-title(v-text="d.title")

                v-list-item.px-4
                  v-list-item-content
                    pre.mb-0(v-if="d.nextline" style="font-family: Arial; font-size: 16px; line-height: 1.4;") {{d.details}}
                    p.mb-0(v-else) {{food.description == '' ?  food.name + d.details : food.description}}

    v-footer.white.rounded-t-xl(absolute elevation="4")
      v-col(class="text-center" cols="6")
        v-card.quantity-card.my-2.rounded-xl.d-flex.align-center.justify-center(outlined)
          p.mb-0 Stock Left: {{food.quantity}}
      v-col(class="text-center" cols="6")
        w-button.bottom-nav--button(
          :label="'Edit Item'"
          block
          dark
          :color="$vuetify.theme.themes.light.primary"
          @click="goToEditPage"
        )
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

import UpperTitle from '../../components/UpperTitle.vue'
import WIcon from '../../components/componenets-custom/WIcon.vue'
import WButton from '../../components/componenets-custom/WButton.vue'

export default {
  name: 'FoodDetailsPage',
  components: {
    UpperTitle,
    WIcon,
    WButton
  },
  layout: 'welcome',
  asyncData ({ params }) {
    // Access the parameters in the page component
    const foodId = params.productId
    const food = params.product

    return { foodId, food }
  },
  data () {
    return {
      foodId: null,
      food: null,
      search: null,
      offsetTop: 0,
      description: [
        {
          action: 'mdi-silverware-fork-knife',
          active: true,
          details: ' are nutritious. It may be good for weight loss. Tomatoes may be good for your heart. As part of a healtful and varied diet.',
          title: 'Product details',
          nextline: false
        },
        {
          action: 'mdi-silverware-fork-knife',
          active: false,
          details: 'Calories: 261-593 kcal \nTotal fat: 13-21.4g\nSaturated fat: 7.6 g\nCarbohydrates: 29-81.4 g\nProtein: 12-18.6 g\nSodium: 838 mg\nCholesterol: 76 mg',
          nextline: true,
          title: 'Nutritions'
        }
      ]
    }
  },
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollYClass'
    }),
    foodLogo () {
      return require(`../../assets/food/${this.food.src}.jpg`)
    },
    foodImg () {
      return require(`../../assets/food/${this.food.src}.jpg`)
    }
  },
  methods: {
    ...mapActions({
    }),
    onScroll (e) {
      this.offsetTop = e.target.scrollTop
    },
    searchBy (newValue) {
      this.search = newValue
    },
    goBackToProductsPage () {
      this.$router.push('/products')
    },
    formatDate (date) {
      const todayFormat = 'HH:mm a'
      const otherDayFormat = 'dd MMM yyyy, HH:mm a'

      const formattedDate = this.$dateFns.isToday(date)
        ? 'Today, ' + this.$dateFns.format(date, todayFormat)
        : this.$dateFns.format(date, otherDayFormat)

      return formattedDate
    },
    goToEditPage () {
      this.$router.push({ name: 'products-edit-productId', params: { productId: this.foodId, product: this.food } })
    }
  }
}
</script>

<style scoped>
:deep(.scroll) {
  overflow-x: hidden;
  overflow-y: auto;
  width: 100% !important;
}
.v-avatar {
  filter: drop-shadow(0px 0px 4px rgba(0, 0, 0, 0.3));
}

.on-top {
  position: absolute;
  top: 0;
  width: 100%;
  z-index: 100;
  border-radius: 0 0 25px 25px;
}

.on-top-200 {
  position: absolute;
  top: 0;
  width: 100%;
  z-index: 100;
  background-color: white;
}

.img-on-top {
  z-index: 99;
}

.second-on-top {
  z-index: 98;
}

.absolute-position {
  border-radius: 0 0 60% 60%;
  background: #FAAF08;
  position: absolute;
  width: 100%;
}

.negative-margin {
  margin-bottom: -65px;
}

.text-decoration-underline {
  text-underline-offset: 3px;
}

.v-chip.v-chip--outlined.v-chip.v-chip {
  background-color: white !important;
  border-width: 2px;
  font-weight: 500;
  font-size: 13px;
  height: 20px
}

.bottom-gray {
  border-bottom: 1px solid lightgray;
}

.quantity-card {
  border: 2px solid #FAAF08;
  height: 48px;
}

.v-footer {
  z-index: 100 !important;
}

</style>
