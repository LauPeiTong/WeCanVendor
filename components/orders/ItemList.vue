<template lang="pug">
.item-list.py-6.px-2
  v-item-group.pt-2.pb-8
    template(v-for='item in items')
      v-row.d-flex.flex-column.px-4.pb-4.justify-center
        v-card.rounded-lg.pr-4(
            @click="editOrder(item)"
            outlined
          )
          v-row
            v-col.pr-0(:cols="3")
              v-img.rounded-lg.ma-2.rounded-circle(:src="require(`../../assets/food/noitem.png`)" width="60" height="60" v-if="item.customer.image_url == '' || item.customer.image_url == null")
              v-img.rounded-lg.ma-2.rounded-circle(:src="item.customer.image_url" width="60" height="60" v-else)
            v-col.py-2.d-flex.flex-column.px-0(:cols="9")
              p.secondary--text.font-weight-medium.mb-0.pt-4.pr-2.text-capitalize {{item.customer.username}}
              p.mb-0.caption {{item.delivery_or_pickup == 'Delivery' ?  'Deliver to: ' + item.customer.address : 'Self Pick-up'}}
              p.caption.darkGrey--text.font-weight-light.mb-0.pb-4 At {{formatDate(new Date(item.created_at))}}
              v-row.d-flex.flex-row.justify-space-between.mb-4.pl-2.pr-4
                v-chip.mt-auto.mr-2.rounded-xl(outlined :color="$vuetify.theme.themes.light.green" v-if="item.status === 'Completed'") {{item.status}}
                v-chip.mt-auto.mr-2.rounded-xl(outlined :color="$vuetify.theme.themes.light.danger" v-else-if="item.status === 'Cancelled'") {{item.status}}
                v-chip.mt-auto.mr-2.rounded-xl(outlined :color="$vuetify.theme.themes.light.primary" v-else) {{item.status}}
                p.mb-0.primary--text {{$formatCurrency(item.total_price)}}
    v-dialog(
      v-model="dialog"
      max-width="350"
      v-if="selectedItem"
    )
      v-card.rounded-xl
        v-img(height="160" :src="require('../../assets/food/edit.png')")
        v-card-title.text-h5 #Order{{selectedItem?.id}}
        v-card-text
          p.mb-0.secondary--text Order Details:
          p.mb-0.caption {{selectedItem?.delivery_or_pickup == 'Delivery' ?  'Deliver to: ' + selectedItem?.customer.address : 'Self Pick-up'}}
          p.caption At {{formatDate(new Date(selectedItem?.created_at))}}

          p.mb-0.secondary--text Customer:
          v-row.align-center
            v-col.pr-0(cols="2")
              v-img.rounded-lg.ma-2.rounded-circle(:src="selectedItem?.customer.image_url" width="25" height="25" v-if="selectedItem?.customer.image_url == '' || selectedItem?.customer.image_url == null")
              v-img.rounded-lg.ma-2.rounded-circle(:src="selectedItem?.customer.image_url" width="25" height="25" v-else)
            v-col.pl-0(cols="10")
              p.text-capitalize.ml-2.mb-0 {{selectedItem?.customer.username}}
              p.text-capitalize.ml-2.mb-0.caption {{selectedItem?.customer.phone}}
        v-divider.mb-4
        v-card-text.d-flex.justify-space-between.align-end
          .d-grid
            p.mb-0.secondary--text Total Payment:
            h3.primary--text {{$formatCurrency(selectedItem?.total_price)}}
          a.text-decoration-underline View Details
        v-divider.mb-4
        v-card-text
          p.mb-0.secondary--text Order Status:
          v-select.outlined-field(
            :items="statusOptions"
            v-model="status"
            dense
            rounded
          )

        v-card-actions.pb-4
          v-spacer
          v-btn(
            color="gray darken-1"
            text
            @click="dialog = false"
          ) Cancel
          v-btn.rounded-xl(
            color="primary"
            elevation="0"
            @click="updateOrderStatus"
          ) Update Status
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import WCard from '../componenets-custom/WCard.vue'

export default {
  name: 'ItemList',
  components: {
    WCard
  },
  props: {
    items: {
      type: Array || Object,
      default: null
    }
  },
  data () {
    return {
      statusOptions: ['Pending', 'Processing', 'To Receive', 'Completed', 'Cancelled'],
      status: null,
      dialog: false,
      selectedItem: null
    }
  },
  computed: {
    ...mapGetters({
      widthX: 'screen/getWidthClass'
    })
  },
  methods: {
    ...mapActions({
    }),
    formatDate (date) {
      const todayFormat = 'HH:mm a'
      const otherDayFormat = 'dd MMM yyyy, HH:mm a'

      const formattedDate = this.$dateFns.isToday(date)
        ? 'Today, ' + this.$dateFns.format(date, todayFormat)
        : this.$dateFns.format(date, otherDayFormat)

      return formattedDate
    },
    goToOrderDetailsPage (item) {
      // this.$router.push({ name: 'orders-orderId', params: { orderId: item.id, order: item } })
    },
    editOrder (item) {
      this.selectedItem = item
      this.status = this.selectedItem.status
      this.dialog = true
    },
    updateOrderStatus () {
      this.dialog = false
      this.$emit('updateOrder', { selectedItem: this.selectedItem, status: this.status })
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
}

:deep(.outlined-field .v-input__slot) {
  border: solid 2px #FAAF08 !important;
  background-color: transparent !important;
  height: 38px;
}
</style>
