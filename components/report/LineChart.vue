<template lang="pug">
v-card.rounded-lg.pa-2.text-center(outlined v-if="category")
  p.text-h6.font-weight-medium Sales per Day
  ApexCharts(type="line" :options="category" :series="series")
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'UpperTitle',
  components: {
    VueApexCharts
  },
  props: {
    series: {
      type: Array,
      default: null
    },
    category: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
      chartOptions: null
    }
  },
  computed: {
    ...mapGetters({
    })
  },
  methods: {
    ...mapActions({
    }),
    emitBack () {
      this.$emit('goBack')
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
