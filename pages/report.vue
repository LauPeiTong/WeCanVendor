<template lang="pug">
.fill-height.report-page.pa-0.ma-0.full-width
  v-row.pa-0.ma-0.upper-row
    upper-title.ma-0(:icon="'heart'" :title="'Reports'")
  .scroll.scrollbar-hide.ma-0.justify-top.align-center(:style="scrollSize")
    v-row.px-4
      v-col(cols="6")
        v-card.rounded-lg.pa-2.text-center(outlined)
          .d-flex.align-center.justify-center
            v-icon.mb-1.mr-1(:color="$vuetify.theme.themes.light.success") mdi-clipboard-text
            span.text-h6.success--text.font-weight-bold {{$formatCurrency(orders?.total_count)}}
          p.mb-0.font-weight-regular Total Order
      v-col(cols="6")
        v-card.rounded-lg.pa-2.text-center(outlined)
          .d-flex.align-center.justify-center
            v-icon.mb-1.mr-1(:color="$vuetify.theme.themes.light.tertiary") mdi-hand-coin
            span.text-h6.tertiary--text.font-weight-bold {{$formatCurrency(orders?.total_amount)}}
          p.mb-0.font-weight-regular Total Sales
      v-col(cols="12")
        donut-chart(:items="donutChartItems")
      v-col(cols="12")
        line-chart.mb-12(:series="barChartItems" :category="barChartCategory")
</template>

<script>
import { mapGetters } from 'vuex'

import UpperTitle from '../components/UpperTitle.vue'
import WSearchBar from '../components/componenets-custom/WSearchBar.vue'
import DonutChart from '../components/report/DonutChart.vue'
import LineChart from '../components/report/LineChart.vue'

export default {
  name: 'ReportPage',
  components: {
    UpperTitle,
    WSearchBar,
    DonutChart,
    LineChart
  },
  layout: 'default',
  data () {
    return {
      search: null,
      orders: null,
      donutChartItems: null,
      barChartItems: null,
      barChartCategory: null
    }
  },
  computed: {
    ...mapGetters({
      scrollSize: 'screen/getScrollClass',
      shops: 'home/getShops',
      recommendedJob: 'home/getRecommededJob'
    })
  },
  async mounted () {
    try {
      this.orders = await this.$axios.$get('/api/orders/summary/?day=true&total=true&status=true')
      console.log('Order list: ', this.orders)

      this.donutChartItems = [
        this.orders.Pending.length,
        this.orders.Processing.length,
        this.orders['To Receive'].length,
        this.orders.Completed.length,
        this.orders.Cancelled.length
      ]

      this.barChartCategory = {
        chart: {
          height: 350,
          type: 'line'
        },
        stroke: {
          width: 5,
          curve: 'smooth'
        },
        xaxis: {
          type: 'datetime',
          categories: Object.keys(this.orders.daily_totals),
          tickAmount: 10
        },
        fill: {
          type: 'gradient',
          gradient: {
            shade: 'dark',
            gradientFromColors: ['#Fa812f'],
            gradientToColors: ['#009966'],
            shadeIntensity: 1,
            type: 'horizontal',
            opacityFrom: 1,
            opacityTo: 0.5,
            stops: [0, 100, 100, 100]
          }
        },
        yaxis: {
          min: -10,
          max: 40
        }
      }
      this.barChartItems = [{ name: 'Sales', data: Object.values(this.orders.daily_totals) }]
      console.log(this.barChartCategory)
      console.log(this.barChartItems)
    } catch (e) {
      console.log('Fail to get order list: ', e)
    }
  },
  methods: {
    searchBy (newValue) {
      this.search = newValue
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

</style>
