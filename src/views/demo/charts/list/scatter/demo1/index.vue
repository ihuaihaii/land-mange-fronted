<template>
  <d2-container :filename="filename">
    <template slot="header">单维度多指标</template>
    <div class="inner">
      <ve-scatter :data="chartData" v-bind="pubSetting"></ve-scatter>
    </div>
    <template slot="footer">
      <d2-link-btn title="更多示例和文档" link="https://v-charts.js.org"/>
    </template>
  </d2-container>
</template>

<script>
import list from '@/views/demo/charts/list/_mixin/list.js'
export default {
  mixins: [
    list
  ],
  data () {
    return {
      filename: __filename,
      chartData: {
        columns: ['日期', '访问用户', '下单用户', '年龄'],
        rows: [
          { '日期': '1/1', '访问用户': 123, '年龄': 3, '下单用户': 1244 },
          { '日期': '1/2', '访问用户': 1223, '年龄': 6, '下单用户': 2344 },
          { '日期': '1/3', '访问用户': 7123, '年龄': 9, '下单用户': 3245 },
          { '日期': '1/4', '访问用户': 4123, '年龄': 12, '下单用户': 4355 },
          { '日期': '1/5', '访问用户': 3123, '年龄': 15, '下单用户': 4564 },
          { '日期': '1/6', '访问用户': 2323, '年龄': 20, '下单用户': 6537 }
        ]
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.inner {
  position: absolute;
  top: 20px;
  right:  20px;
  bottom: 20px;
  left: 20px;
}
</style>
