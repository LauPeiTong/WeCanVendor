<template lang="pug">
v-card.rounded-lg.pa-2.text-center(outlined v-if="items")
  p.text-h6.font-weight-medium Order Status
  ApexCharts(type="donut" :options="donutChartOptions" :series="items")
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
    items: {
      type: Array,
      default: null
    }
  },
  data () {
    return {
      donutChartOptions: {
        chart: {
          type: 'donut'
        },
        legend: {
          show: true
        },
        labels: ['Pending', 'Processing', 'To Receive', 'Completed', 'Cancelled'],
        colors: ['#333333', '#FA812F', '#FAAF08', '#009966', '#C82F2F'],
        plotOptions: {
          pie: {
            expandOnClick: true,
            donut: {
              labels: {
                show: true,
                total: {
                  showAlways: true,
                  show: true,
                  color: '#333333',
                  fontSize: '16px',
                  fontWeight: '600',
                  formatter: function (value) {
                    const t = value.globals.series.reduce((a, b) => a + b, 0)
                    return t.toString() + ' Orders'
                  }
                },
                value: {
                  color: '#333333',
                  fontSize: '20px',
                  fontWeight: '600'
                }
              }
            }
          }
        },
        responsive: [{
          breakpoint: 480,
          options: {
            chart: {
              width: '100%'
            },
            legend: {
              position: 'bottom'
            }
          }
        }],
        dataLabels: {
          enabled: false
        }
      }
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
