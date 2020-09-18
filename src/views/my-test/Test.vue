<template>
  <page-header-wrapper>
<a-card>
  <div id="c1">111</div>

</a-card>

  </page-header-wrapper>
</template>

<script>
import { Chart } from '@antv/g2'

export default {
  data() {
    return {
      chartData: [
        { item: '事例一', count: 50, percent: 0.4 },
        { item: '事例二', count: 21, percent: 0.21 },
        { item: '事例三', count: 17, percent: 0.17 },
        { item: '事例四', count: 13, percent: 0.13 },
        { item: '事例五', count: 9, percent: 0.09 },
      ],
    }
  },
  mounted() {
    this.getChart()
  },
  methods: {
    getChart() {
      const chart = new Chart({
        container: 'c1',
        width: 400,
        height: 350,
      })
      chart.source(this.chartData)
      chart.scale('percent', {
        formatter: (val) => {
          val = val * 100 + '%'
          return val
        },
      })
      chart.coordinate('theta', {
        radius: 0.75,
        innerRadius: 0.6,
      })
      chart.tooltip({
        showTitle: false,
        showMarkers: false,
        itemTpl:
          '<li class="g2-tooltip-list-item"><span style="background-color:{color};" class="g2-tooltip-marker"></span>{name}: {value}</li>',
      })
      // 辅助文本
      chart.annotation().text({
        position: ['50%', '50%'],
        content: '主机',
        style: {
          fontSize: 14,
          fill: '#8c8c8c',
          textAlign: 'center',
        },
        // offsetY: -20,
      })
      chart
        .interval()
        .adjust('stack')
        .position('percent')
        .color('item')
        .label('count', (count) => {
          return {
            content: (data) => {
              return `${data.item}: ${count}`
            },
          }
        })
        .tooltip('item*percent', (item, percent) => {
          percent = percent * 100 + '%'
          return {
            name: item,
            value: percent,
          }
        })

      chart.interaction('element-active')

      chart.render()
    },
  },
}
</script>

<style>
</style>