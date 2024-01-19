<template lang="pug">
.activity-list.pt-2
  v-tabs.scroll-x(
    background-color="white"
    :color="$vuetify.theme.themes.light.primary"
    :style="scrollSize"
    grow
  )
    v-tab(v-for="item in tabs" :key="item.name" @click="updateList(item.name)") {{item.name}}
  item-list(:items="items")
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import WIcon from '../componenets-custom/WIcon.vue'
import ItemList from './ItemList.vue'

export default {
  name: 'FoodList',
  components: {
    WIcon,
    ItemList
  },
  props: {
  },
  data () {
    return {
      food: null,
      tabs: [
        { id: 1, name: 'Near Expiry' },
        { id: 2, name: 'Within Shelf Life' },
        { id: 3, name: 'Expired' }
      ],
      items: null
    }
  },
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollXClass'
      // foods: 'food/getFoods'
    })
  },
  async mounted () {
    try {
      this.food = await this.$axios.$get('/api/products/?recommended=true')
      console.log('Food status data: ', this.food)

      this.updateList('Near Expiry')
    } catch (e) {
      console.log('Fail to get food items: ', e)
    }
  },
  methods: {
    ...mapActions({
    }),
    updateList (status) {
      this.items = this.food[status]
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
