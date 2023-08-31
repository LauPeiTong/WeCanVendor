<template lang="pug">
.addproduct-page.pa-0.ma-0
  v-row.pa-0.ma-0.upper-row
      upper-title.ma-0(:title="'Add Product'" :title-class="'dark-background'" @goBack="goBackToHomePage" :back="true")
      w-search-bar.ma-0(@change="searchBy")
  .scroll.ma-0.justify-top.align-center(:style="scrollSize")
    v-container.pa-4.pb-16
      v-form(ref="form")
        v-file-input.mt-4(label="Add Product Image" prepend-icon="mdi-camera" outlined)

        v-divider.mb-4(:thickness="5")

        v-text-field.mt-4(:rules="rules" label="Product Name" placeholder = "Enter product name" filled counter="120" clearable)
          v-model="product name"

        v-textarea.mt-4(:rules="rules" label="Product Description" placeholder = "Enter product name" filled counter="3000" clearable auto-grow)
          v-model="product description"

        v-divider.mt-4(:thickness="5")

        .text-h6.mt-2
          |Discount

        v-container.pl-0
            v-row.pa-0(justify = "center", align = "center")
              v-col
                v-text-field(suffix = "%" outlined )
              v-col-1.pb-8.pr-2
                .p
                  | Until
              v-col-2
                v-dialog(ref="dialog"
                  v-model="modal"
                  :return-value.sync="date"
                  persistent
                  width="290px")
                  <template v-slot:activator="{ on, attrs }">
                      v-text-field(
                        v-model="date"
                        append-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                        outlined
                      )
                  </template>
                  v-date-picker(
                    v-model="date"
                    scrollable
                  )
                    v-btn(text color="primary" @click="modal = false")
                      |Cancel
                    v-btn(text color="primary" @click="$refs.dialog.save(date)")
                      |OK
        v-divider.mb-8(:thickness="5")
        v-btn.mt-2(block
          color="primary"
          rounded
          @click="goBackToHomePage(home)")
          | Add Product
</template>
<script>
import { mapGetters } from 'vuex'

export default {
  data: () => ({
    date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
    menu: false,
    modal: false,
    menu2: false
  }),
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollClass'
    })
  },
  methods: {
    goBackToHomePage () {
      this.$router.push('/homeproduct')
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
</style>
