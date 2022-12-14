<template>
  <div class="dashboard-editor-container">

    <panel-group @handleSetLineChartData="handleSetLineChartData" />

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <div class="chart-wrapper">
        <bar-chart />
      </div>
    </el-row>
  </div>

</template>

<script>
import PanelGroup from './components/PanelGroup'
import LineChart from './components/LineChart'
import BarChart from './components/BarChart'

const lineChartData = {
  email_sent: {
    actualData: [0, 3, 0, 0, 0, 0, 0]
  },
  click_total: {
    expectedData: [200, 192, 120, 144, 160, 130, 140],
    actualData: [0, 0, 1, 1, 0, 0, 0]
  },
  opens: {
    expectedData: [80, 100, 121, 104, 105, 90, 100],
    actualData: [0, 0, 3, 3, 0, 0, 0]
  },
  unique_opens: {
    expectedData: [130, 140, 141, 142, 145, 150, 160],
    actualData: [0, 0, 1, 1, 0, 0, 0]
  },
  open_rate: {
    expectedData: [100, 100, 100, 100, 100, 100, 100],
    actualData: [0, 0, 75, 75, 0, 0, 0]
  },
  empty: {
    actualData: [0, 0, 0, 0, 0, 0, 0]
  }
}

export default {
  name: 'DashboardAdmin',
  components: {
    PanelGroup,
    LineChart,
    BarChart
  },
  data() {
    return {
      lineChartData: lineChartData.newVisitis
    }
  },
  methods: {
    handleSetLineChartData(type) {
      this.lineChartData = lineChartData[type]
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  background-color: rgb(240, 242, 245);
  position: relative;

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}

</style>
