<template>
  <el-row :gutter="40" class="panel-group">
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('email_sent')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="peoples" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Emails Sent
          </div>
          <count-to :start-val="0" :end-val="emails_sent" :duration="2600" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('click_total')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="peoples" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Click Total
          </div>
          <count-to :start-val="0" :end-val="clicks" :duration="2600" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('opens')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="email" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Opens
          </div>
          <count-to :start-val="0" :end-val="opens_total" :duration="3000" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('unique_opens')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="email" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Unique Opens
          </div>
          <count-to :start-val="0" :end-val="unique_opens" :duration="3200" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('open_rate')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="guide" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Open Rate
          </div>
          <count-to :start-val="0" :end-val="open_rate" :duration="3600" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('empty')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="guide" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Sub Rate
          </div>
          <count-to :start-val="0" :end-val="sub_rate" :duration="3600" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('empty')">
        <div class="card-panel-icon-wrapper icon-people">
          <svg-icon icon-class="guide" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            Unsub Rate
          </div>
          <count-to :start-val="0" :end-val="unsub_rate" :duration="3600" class="card-panel-num" />
        </div>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import CountTo from 'vue-count-to'

export default {
  name: 'Report',
  components: {
    CountTo
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
      clicks: 0,
      emails_sent: 0,
      opens_total: 0,
      unique_opens: 0,
      open_rate: 0,
      sub_rate: 0,
      unsub_rate: 0
    }
  },
  created() {
    this.loadReports()
  },
  methods: {
    handleSetLineChartData(type) {
      this.$emit('handleSetLineChartData', type)
    },

    loadReports() {
      this.isLoading = true
      this.error = null
      fetch('http://127.0.0.1:5000/reports').then((response) => {
        if (response.ok) {
          return response.json()
        }
      }).then((data) => {
        // console.log(data);
        this.isLoading = false
        const results = []
        for (const id in data['reports']) {
          results.push(data['reports'][id])
        }
        this.results = results
        console.log(results)
        this.clicks = results[0].clicks.clicks_total
        this.emails_sent = results[0].emails_sent
        this.opens_total = results[0].opens.opens_total
        this.unique_opens = results[0].opens.unique_opens
        this.open_rate = results[0].list_stats.open_rate
        this.sub_rate = results[0].list_stats.sub_rate
        this.unsub_rate = results[0].list_stats.unsub_rate
      })
        .catch((error) => {
          console.log(error)
          this.isLoading = false
          this.error = 'Failed to fetch data. Please try again later'
        })
    }
  }
}
</script>

<style lang="scss" scoped>
.panel-group {
  margin-top: 18px;

  .card-panel-col {
    margin-bottom: 32px;
  }

  .card-panel {
    height: 108px;
    cursor: pointer;
    font-size: 12px;
    position: relative;
    overflow: hidden;
    color: #666;
    background: #fff;
    box-shadow: 4px 4px 40px rgba(0, 0, 0, .05);
    border-color: rgba(0, 0, 0, .05);

    &:hover {
      .card-panel-icon-wrapper {
        color: #fff;
      }

      .icon-people {
        background: #40c9c6;
      }

      .icon-message {
        background: #36a3f7;
      }

      .icon-money {
        background: #f4516c;
      }

      .icon-shopping {
        background: #34bfa3
      }
    }

    .icon-people {
      color: #40c9c6;
    }

    .icon-message {
      color: #36a3f7;
    }

    .icon-money {
      color: #f4516c;
    }

    .icon-shopping {
      color: #34bfa3
    }

    .card-panel-icon-wrapper {
      float: left;
      margin: 14px 0 0 14px;
      padding: 16px;
      transition: all 0.38s ease-out;
      border-radius: 6px;
    }

    .card-panel-icon {
      float: left;
      font-size: 48px;
    }

    .card-panel-description {
      float: right;
      font-weight: bold;
      margin: 26px;
      margin-left: 0px;

      .card-panel-text {
        line-height: 18px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 16px;
        margin-bottom: 12px;
      }

      .card-panel-num {
        font-size: 20px;
      }
    }
  }
}

@media (max-width:550px) {
  .card-panel-description {
    display: none;
  }

  .card-panel-icon-wrapper {
    float: none !important;
    width: 100%;
    height: 100%;
    margin: 0 !important;

    .svg-icon {
      display: block;
      margin: 14px auto !important;
      float: none !important;
    }
  }
}
</style>
