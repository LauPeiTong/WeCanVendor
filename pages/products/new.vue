<template lang="pug">
.fill-height.edit-product-page.pa-0.ma-0
  v-row.pa-0.ma-0.upper-row
    upper-title.ma-0(:title="'Edit Product'" @goBack="goBackToProductPage" :back="true")
  .scroll.ma-0.justify-top.align-center.full-width(:style="scrollSize")
    v-row
      v-col.px-8(ocls="12")
        v-form(ref="form")
          v-file-input.mt-4.outlined-field(
            label="Add Product Image"
            accept="image/png, image/jpeg, image/bmp"
            prepend-icon="mdi-camera"
            v-model="image"
            rounded
            hide-details="True"
            filled
          )

          v-divider.my-4

          p.mb-0.font-weight-medium Product Name
          v-text-field.filled-field(
            label="Exp: Sushi"
            placeholder = "Enter product name"
            filled
            rounded
            dense
            counter="120"
            clearable
            v-model="name"
          )

          v-divider.my-4

          p.mb-0.font-weight-medium Category
          v-text-field.filled-field(
            label="Exp: Lunch"
            placeholder = "Enter product category"
            filled
            rounded
            dense
            counter="120"
            clearable
            v-model="category"
          )

          v-divider.mb-4

          p.mb-0.font-weight-medium Product Description
          v-textarea.filled-field(
            label="Exp: Sushi is healthy"
            placeholder = "Enter product description"
            filled
            counter="3000"
            clearable
            dense
            rounded
            auto-grow
            v-model="description"
          )

          v-divider.mb-4

          p.mb-0.font-weight-medium Nutrients
          v-textarea.filled-field(
            label="Exp: Sushi has protein"
            placeholder = "Enter product description"
            filled
            counter="3000"
            clearable
            dense
            rounded
            auto-grow
            v-model="nutrients"
          )

          v-divider.mb-4

          v-row.align-center
            v-col(cols="6")
              p.mb-0.font-weight-medium Price
            v-col(cols="6")
              v-text-field.outlined-field(
                prefix="RM"
                placeholder = "00.00"
                v-model="price"
                type="number"
                hide-details="True"
                rounded
                filled
              )

          v-divider.my-4

          v-row.align-center
            v-col(cols="6")
              p.mb-0.font-weight-medium Stock
            v-col(cols="6")
              v-text-field.outlined-field(
                suffix="Item"
                placeholder = "0"
                v-model="quantity"
                type="number"
                hide-details="True"
                rounded
                filled
              )

          v-divider.my-4

          v-row.align-center
            v-col(cols="6")
              p.mb-0.font-weight-medium Discount
            v-col(cols="6")
              v-text-field.outlined-field(
                suffix="%"
                placeholder = "0.00"
                v-model="discount"
                type="number"
                hide-details="True"
                rounded
                filled
              )

          v-divider.my-4

          v-row.align-center
            v-col(cols="6")
              p.mb-0.font-weight-medium Expierd Date
            v-col(cols="6")
              v-dialog(
                ref="dialog"
                v-model="modal"
                :return-value.sync="expiredDate"
                persistent
                width="290px"
              )
                template(v-slot:activator="{ on, attrs }")
                  v-text-field.outlined-field(
                    v-model="expiredDate"
                    append-icon="mdi-calendar"
                    readonly
                    hide-details="True"
                    v-bind="attrs"
                    v-on="on"
                    rounded
                    filled
                  )
                v-date-picker(
                  v-model="expiredDate"
                  scrollable
                )
                  v-btn(text color="primary" @click="modal = false")
                    |Cancel
                  v-btn(text color="primary" @click="$refs.dialog.save(expiredDate)")
                    |OK

          v-divider.my-4

    v-footer.white.rounded-t-xl(absolute elevation="4")
      v-col(class="text-center" cols="12")
        w-button.bottom-nav--button(
          :label="'Add Item'"
          block
          dark
          :color="$vuetify.theme.themes.light.primary"
          @click="addItem"
        )
</template>
<script>
import { mapGetters } from 'vuex'

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
  data: () => ({
    expiredDate: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
    modal: false,
    image: null,
    image_url: 'https://images.deliveryhero.io/image/fd-my/Products/1477196593.jpg',
    category: '',
    name: '',
    price: 0,
    nutrients: '',
    description: '',
    discount: 0,
    quantity: 0
  }),
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollClass'
    })
  },
  methods: {
    goBackToProductPage () {
      this.$router.push('/products')
    },
    async addItem () {
      const newProductData = {
        vendor: this.$store.getters['auth/getAuthId'], // Replace with the actual user ID
        name: this.name,
        image_url: this.image_url,
        description: this.description,
        category: this.category,
        original_price: this.price,
        discount: this.discount,
        quantity: this.quantity,
        expired_date: this.formatExpiredDate(this.expiredDate),
        nutrients: this.nutrients
      }

      //   console.log('Product data: ', newProductData)

      const product = await this.$axios.post('/api/products/', newProductData)
      console.log('New product:', product)

      this.$router.push('/products')
    },
    formatExpiredDate (date) {
      const currentDate = new Date(date)
      // Set the time to 23:59:59
      currentDate.setHours(23, 59, 59)
      // Adjust the date based on the timezone offset
      currentDate.setTime(currentDate.getTime() - currentDate.getTimezoneOffset() * 60000)

      return currentDate.toISOString().substr(0, 19)
    }
  }
}
</script>

<style lang ="scss" scoped>
:deep(.scroll) {
  overflow-x: hidden;
  overflow-y: auto;
  width: 100% !important;
}

:deep(.v-menu__content) {
  z-index: 50;
}

:deep(.filled-field .v-input__slot) {
  background-color: rgba(250, 175, 8, 0.2) !important;
}

:deep(.outlined-field .v-input__slot) {
  border: solid 2px #FAAF08 !important;
  background-color: transparent !important;
}

:deep(.v-input__icon .v-icon) {
  color: #FAAF08 !important;
}

.v-footer {
  z-index: 100 !important;
}

</style>
