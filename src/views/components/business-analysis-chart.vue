<template>
  <d2-container :filename="filename">
    <template slot="header">
      <el-form
            :inline="true"
            :model="form"
            ref="form"
            style="margin-bottom: 10px">
            <el-form-item label="镇名">
                <el-select v-model="form.town" placeholder="请选择">
                    <el-option
                            v-for="item in townList"
                            :key="item.id"
                            :label="item.name"
                            :value="item.id">
                    </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="村名">
                <el-select v-model="form.village" placeholder="请选择">
                            <el-option
                                v-for="item in villageList"
                                :key="item.id"
                                :label="item.name"
                                :value="item.name">
                            </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="年份">
                <el-select v-model="form.year" placeholder="请选择">
                            <el-option
                                v-for="item in yearList"
                                :key="item.id"
                                :label="item.name"
                                :value="item.name">
                            </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="统计类型">
                <el-select @change="show" placeholder="请选择">
                            <el-option
                                v-for="item in analysisTypes"
                                :key="item.id"
                                :label="item.name"
                                :value="item.id">
                            </el-option>
                </el-select>
            </el-form-item>
            <el-button
                  type="primary"
                  style="float:right"
                  @click="search">
                  查询
                </el-button>
        </el-form>
    </template>
    <el-tabs
            v-model="editableTabsValue"
           >
            <el-tab-pane
                style="height:800px"
                label="数据分析"
                name="first"
            >
              <div class="inner" >
                <ve-pie :data="chartData" v-if="showChart == 0"></ve-pie>
                <ve-histogram :data="histogramData" v-if="showChart == 1" v-bind="pubSetting"></ve-histogram>
              </div>
            </el-tab-pane>
            <el-tab-pane
                style="height:800px"
                label="数据展示"
                name="second"
            >
                <BusinessAnalysisData
                  :templateData="templateData"
                  :loading ="loading" />
                <div class="pagination">
                    <el-pagination background @current-change="paginationCurrentChange" layout="prev, pager, next" :total="pagination.total">
                    </el-pagination>
                </div>
            </el-tab-pane>
        </el-tabs>
    <template slot="footer">
      <d2-link-btn title="更多示例和文档" link="https://v-charts.js.org"/>
    </template>
  </d2-container>
</template>

<script>
import list from '@/views/demo/charts/list/_mixin/list.js'
import BusinessAnalysisData from '@/views/components/business-analysis-data'
import { getBusinessAnalysisPage } from '@api/business.mange.js'

export default {
  mixins: [
    list
  ],
  components: {
    BusinessAnalysisData
  },
  data () {
    return {
      form: {
        town: '1',
        village: '',
        year: '2019'
      },
      showChart: 0,
      townList: [],
      yearList: [],
      analysisTypes: [
        { 'id': 0, 'name': '经营者统计' },
        { 'id': 1, 'name': '经营类型统计' }
      ],
      pagination: {
        page: 0,
        size: 10,
        total: 0
      },
      loading: true,
      templateData: [],
      villageList: [],
      editableTabsValue: 'first',
      filename: __filename,
      chartData: {
        columns: ['日期', '访问用户'],
        rows: [
          { '日期': '1/1', '访问用户': 1393 },
          { '日期': '1/2', '访问用户': 3530 },
          { '日期': '1/3', '访问用户': 2923 },
          { '日期': '1/4', '访问用户': 1723 },
          { '日期': '1/5', '访问用户': 3792 },
          { '日期': '1/6', '访问用户': 4593 }
        ]
      },
      histogramData: {
        columns: ['日期', '访问用户', '下单用户', '下单率'],
        rows: [
          { '日期': '1/1', '访问用户': 1393, '下单用户': 1093, '下单率': 0.32 },
          { '日期': '1/2', '访问用户': 3530, '下单用户': 3230, '下单率': 0.26 },
          { '日期': '1/3', '访问用户': 2923, '下单用户': 2623, '下单率': 0.76 },
          { '日期': '1/4', '访问用户': 1723, '下单用户': 1423, '下单率': 0.49 },
          { '日期': '1/5', '访问用户': 3792, '下单用户': 3492, '下单率': 0.323 },
          { '日期': '1/6', '访问用户': 4593, '下单用户': 4293, '下单率': 0.78 }
        ]
      }
    }
  },
  mounted () {
    this.fetchData()
    this.townList = this.global.townList
    this.villageList = this.global.villageList
    this.yearList = this.global.yearList
  },
  methods: {
    // 分页导航
    paginationCurrentChange (currentPage) {
      this.pagination.page = currentPage - 1
      this.fetchData()
    },
    fetchData () {
      this.loading = true
      getBusinessAnalysisPage({
        ...this.pagination,
        ...this.form
      }).then(res => {
        this.templateData = res.content
        this.pagination.total = res.totalElements
        this.loading = false
      }).catch(err => {
        console.log('err', err)
        this.loading = false
      })
    },
    search () {
      this.loading = true
      this.fetchData()
    },
    show (val) {
      console.log(val)
      this.showChart = val
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
