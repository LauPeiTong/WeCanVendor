<template lang="pug">
.order-list.pt-2
  v-tabs.scroll-x(
    background-color="white"
    v-model="activeTab"
    :color="$vuetify.theme.themes.light.primary"
    :style="scrollSize"
    grow
  )
    v-tab(v-for="item in tabs" :key="item.id" @click="updateList(item.name)") {{item.name}}
  item-list(:items="items" @updateOrder="updateOrderStatus")
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import WIcon from '../componenets-custom/WIcon.vue'
import ItemList from '../orders/ItemList.vue'

export default {
  name: 'OrderList',
  components: {
    WIcon,
    ItemList
  },
  props: {
  },
  data () {
    return {
      activeTab: 0,
      orders: [],
      tabs: [
        { id: 0, name: 'Pending' },
        { id: 1, name: 'Processing' },
        { id: 2, name: 'To Receive' },
        { id: 3, name: 'Completed' },
        { id: 4, name: 'Cancelled' }
      ],
      items: null,
      num: 0
    }
  },
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollXClass'
    })
  },
  async mounted () {
    try {
      this.orders = await this.$axios.$get('/api/orders/summary/?status=true')
      console.log('Order list: ', this.orders)
      if (this.items == null) {
        this.updateList('Pending')
      }
    } catch (e) {
      console.log('Fail to get order list: ', e)
    }
  },
  methods: {
    ...mapActions({
    }),
    updateList (status) {
      this.activeTab = this.tabs.filter(i => i.name === status)[0].id
      this.items = this.orders[status]
    },
    async updateOrderStatus (payload) {
      try {
        const newOrder = { ...payload.selectedItem }
        newOrder.status = payload.status
        newOrder.vendor = payload.selectedItem.vendor.id
        newOrder.customer = payload.selectedItem.customer.id

        const response = await this.$axios.put(`/api/orders/${payload.selectedItem.id}/`, newOrder)
        console.log('Order is updated: ', response)

        this.orders = await this.$axios.$get('/api/orders/summary/?status=true')
        console.log('New order list: ', this.orders)

        this.updateList(payload.status)
      } catch (e) {
        console.log('Fail to update order: ', e)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
:deep(.scroll-x) {
  overflow-x: auto !important;
  overflow-y: hidden;
}

:deep(.v-slide-group__prev) {
  min-width: 0px !important;
  flex: 0px;
}

.v-tabs {
  filter: drop-shadow(0px 10px 4px rgba(0, 0, 0, 0.025));
}

.v-tab {
  text-transform: none !important;
}
</style>
