<template>
  <view class="content main">
    <div class="bg">
      <div class="blue">
        <div class="projectWrap">
          <div
            v-if="token"
            class="select-project"
            @click="$utils.toUrl(`/pages/select-project/select-project?projectId=${projectId}`)"
          >
            <b>{{ projectName }}</b
            ><i></i>
          </div>
          <div v-else style="flex: 1; overflow: hidden;"></div>
        </div>
      </div>
      <div class="gray"></div>
    </div>
    <div class="weather-con">
      <div class="weather card">
        <image src="@/static/images/main/weather.png" alt="" />
        <div>
          <header>
            <div class="value">{{ weatherInfo.temperature | formatStr }}°C</div>
            <div class="date">{{ weatherInfo.temperatureTime | formatData }}</div>
          </header>
          <footer>
            <div class="info">
              {{ weatherInfo.weather | formatStr }} {{ weatherInfo.windDirection | formatStr }}
              {{ weatherInfo.windPower | formatStr }}
            </div>
            <div class="location">
              <uni-icons type="location" size="16" color="#666"></uni-icons>{{ weatherInfo.city | formatStr }}
            </div>
          </footer>
        </div>
      </div>
    </div>
    <scroll-view class="scroll-roll" scroll-y>
      <div
        class="main-body"
        :class="{
          'empty-wrapper': list.length === 0
        }"
      >
        <ul class="module-list card">
          <!-- 未登录占位 start -->
          <template v-if="!token">
            <li @click="$utils.toUrl()">
              <image class="list-icon" src="@/static/images/application/Model.png" />
              <p>BIM模型</p>
            </li>
            <li @click="$utils.toUrl()">
              <image class="list-icon" src="@/static/images/application/Schedule.png" />
              <p>进度管理</p>
            </li>
            <li @click="$utils.toUrl()">
              <image class="list-icon" src="@/static/images/application/Quality.png" />
              <p>质量管理</p>
            </li>
            <li @click="$utils.toUrl()">
              <image class="list-icon" src="@/static/images/application/Safe.png" />
              <p>安全管理</p>
            </li>
          </template>
          <!-- 未登录占位 end -->
          <!-- 精装修 start -->
          <li v-if="$hasPower('DecorationProgressSmartDecoration')" @click="$utils.toUrl('/pages/task/progress')">
            <image src="@/static/images//application/DecorationProgressSmartDecoration.png" />
            <p>工序管理</p>
          </li>
          <li
            v-if="$hasPower('ConstructionLogSmartDecoration')"
            @click="$utils.toUrl('/pages/log-SmartDecoration/log')"
          >
            <image src="@/static/images/application/ConstructionLogSmartDecoration.png" />
            <p>施工日志</p>
          </li>
          <!-- <li @click="$utils.toUrl('/pages/bim-model/select-model-room')">
              <image src="@/static/images/main/room-demo.png" />
              <p>样板间预览</p>
            </li> -->
          <!-- 精装修 end -->
          <!-- 开发商 start -->
          <li v-if="$hasPower('ContractDetailMerchants')" @click="$utils.toUrl('/pages/agreement/index')">
            <image src="@/static/images/application/ContractDetailMerchants.png" />
            <p>合同管理</p>
          </li>
          <li v-if="$hasPower('MaterialPlanListView')" @click="$utils.toUrl('/pages/material/planList/index')">
            <image src="@/static/images/application/MaterialPlanListView.png" />
            <p>甲供材计划单</p>
          </li>
          <li
            v-if="$hasPower('procurementPartyDetail') || $hasPower('procurementSupplierDetail')"
            @click="procurementNotice()"
          >
            <image src="@/static/images/application/procurementPartyDetail.png" />
            <p>采购通知</p>
          </li>
          <li v-if="$hasPower('MaterialConfirmCodeDetail')" @click="$utils.toUrl('/pages/material/confirmation/index')">
            <image src="@/static/images/application/MaterialConfirmCodeDetail.png" />
            <p>材料确认单</p>
          </li>
          <!-- 开发商 end -->
          <!-- EPC start -->
          <li v-if="$hasPower('ProjectMeetingEpc')" @click="$utils.toUrl('/pages/project-approval-Epc/meeting-list')">
            <image src="@/static/images/application/ProjectMeetingEpc.png" />
            <p>立项会议</p>
          </li>
          <li v-if="$hasPower('ProjectPlanEpc')" @click="$utils.toUrl('/pages/project-approval-Epc/control-plan-list')">
            <image src="@/static/images/application/ProjectPlanEpc.png" />
            <p>总控计划</p>
          </li>
          <li v-if="$hasPower('TenderListEpc')" @click="$utils.toUrl('/pages/recruit-manage-epc/tender/index')">
            <image src="@/static/images/application/TenderListEpc.png" />
            <p>招标登记</p>
          </li>
          <li v-if="$hasPower('WinningListEpc')" @click="$utils.toUrl('/pages/recruit-manage-epc/winning/index')">
            <image src="@/static/images/application/WinningListEpc.png" />
            <p>中标登记</p>
          </li>
          <li v-if="$hasPower('CompletionEpc')" @click="$utils.toUrl('/pages/application/application-completion-epc')">
            <image src="@/static/images/application/CompletionEpc.png" />
            <p>竣工验收</p>
          </li>
          <li v-if="$hasPower('FundManageEpc')" @click="$utils.toUrl('/pages/application/application-fundManage-epc')">
            <image src="@/static/images/application/FundManageEpc.png" />
            <p>资金管理</p>
          </li>
          <!-- EPC end -->
          <!-- 施工 start -->
          <li v-if="$hasPower('Labour')" @click="$utils.toUrl('/pages/labour/labour-list')">
            <image src="@/static/images/application/Labour.png" />
            <p>劳务管理</p>
          </li>
          <li v-if="$hasPower('ConstructionLog')" @click="$utils.toUrl('/pages/log/log')">
            <image src="@/static/images/application/ConstructionLog.png" />
            <p>施工日志</p>
          </li>
          <li v-if="$hasPower('Model')" @click="toBim()">
            <image src="@/static/images/application/Model.png" />
            <p>BIM模型</p>
          </li>
          <li v-if="$hasPower('Schedule')" @click="$utils.toUrl('/pages/application/application-schedule')">
            <image src="@/static/images/application/Schedule.png" />
            <p>进度管理</p>
          </li>
          <li v-if="$hasPower('Quality')" @click="$utils.toUrl('/pages/application/application-quality')">
            <image src="@/static/images/application/Quality.png" />
            <p>质量管理</p>
          </li>
          <li v-if="$hasPower('Safe')" @click="$utils.toUrl('/pages/application/application-safe')">
            <image src="@/static/images/application/Safe.png" />
            <p>安全管理</p>
          </li>
          <li v-if="$hasPower('Supplies')" @click="$utils.toUrl('/pages/application/application-material?type=0')">
            <image src="@/static/images/application/Supplies.png" />
            <p>物资设备</p>
          </li>
          <li v-if="$hasPower('DataManage')" @click="$utils.toUrl('/pages/document/index')">
            <image src="@/static/images/application/DataManage.png" />
            <p>文档管理</p>
          </li>
          <li v-if="$hasPower('Contract')" @click="$utils.toUrl('/pages/contract/index')">
            <image src="@/static/images/application/Contract.png" />
            <p>合同管理</p>
          </li>
          <!-- 施工 end -->
        </ul>
        <ul v-if="list.length > 0" class="log-list">
          <li v-for="(e, i) in list" :key="i" class="card" :class="{ unread: e.readFlag === 2 }" @click="toDetails(e)">
            <p>
              <span>{{ e.content }}</span>
            </p>
            <p>
              <span class="time">{{ e.createTime | date }}</span>
            </p>
          </li>
        </ul>
        <!-- 无数据空载页 start -->
        <EmptyTemplate v-if="list.length === 0" />
        <!-- 无数据空载页 end -->
      </div>
    </scroll-view>
  </view>
</template>

<script>
import moment from 'moment'
import { mapGetters } from 'vuex'
import { workFlowApi } from '@/api/work-flow'
import { loginApi } from '@/api/login'
import { msgApi } from '@/api/message'
import qualityType from '@/pages/user/setting-items/lib/qualityType'
export default {
  filters: {
    date(val) {
      return moment(val).format('YYYY-MM-DD HH:mm')
    }
  },
  data() {
    return {
      list: [],
      date: moment().format('YYYY-MM-DD')
    }
  },
  computed: {
    projectId() {
      return this.$store.state.project.projectId
    },
    projectName() {
      return this.$store.state.project.projectName
    },
    project() {
      return uni.getStorageSync('project')
    },
    token() {
      return this.$store.state.user.token
    },
    weatherInfo() {
      return this.$store.state.project.weatherInfo
    },
    userId() {
      return this.$store.state.user.userId
    },
    // 待办跳转定义
    taskTodoPower() {
      return this.$store.state.userPower.taskTodoPower
    },
    // 消息类型缓存
    messageTypes() {
      return this.$store.state.project.messageTypes
    }
  },
  onLoad() {
    this.getCommonMessageList()
  },
  onShow() {
    this.init()
  },
  // 下拉刷新
  onPullDownRefresh() {
    this.init()
  },
  methods: {
    // 初始化
    init() {
      if (!uni.getStorageSync('guide')) {
        this.$utils.toUrl('/pages/guide/index', 1)
        uni.setStorageSync('guide', 1)
        return
      }
      // #ifdef H5
      let routePath = this.$route.path
      // #endif
      if (this.$store.state.user.token && this.$store.state.project.projectId) {
        this.$store.commit('user/login', () => {
          this.$store.commit('getPower') // 获取用户授权
          this.getUserList()
          this.getWeather()
          this.getMessage()
        })
      } else {
        this.$store.commit('user/logOut')
      }
      // #ifdef APP-PLUS
      this.checkUpdate()
      // #endif
    },
    // 检查APP是否有更新
    async checkUpdate() {
      let self = this
      let result = await loginApi.appCheckUpdate({
        appTypeCode: 'construction'
      })
      let datas = result.data
      // 获取本地应用资源版本号
      plus.runtime.getProperty(plus.runtime.appid, function (inf) {
        let wgtVer = ''
        wgtVer = inf.version
        if (wgtVer !== datas.versionCode) {
          uni.showModal({
            title: '温馨提示',
            content: '检测到新版本，是否更新？',
            success: e => {
              if (e.cancel) {
                return
              }
              self.hotDown(datas.fileUrl)
            }
          })
        }
      })
    },
    // 热更新APP
    hotDown(wgtUrl) {
      if (!wgtUrl) {
        return
      }
      const installWgt = path => {
        plus.nativeUI.showWaiting('自动更新中...')
        plus.runtime.install(
          path,
          {},
          function () {
            plus.nativeUI.closeWaiting()
            // console.log('安装wgt文件成功！')
            plus.nativeUI.alert('应用资源更新完成！', function () {
              plus.runtime.restart()
            })
          },
          function (e) {
            plus.nativeUI.closeWaiting()
            // console.log('安装wgt文件失败[' + e.code + ']：' + e.message)
            plus.nativeUI.alert('安装wgt文件失败[' + e.code + ']：' + e.message)
          }
        )
      }
      plus.nativeUI.showWaiting('下载wgt文件...')
      plus.downloader
        .createDownload(wgtUrl, { filename: '_doc/update/' }, function (d, status) {
          if (status === 200) {
            // console.log('下载wgt成功：' + d.filename)
            installWgt(d.filename) // 安装wgt包
          } else {
            // '下载wgt失败！'
            plus.nativeUI.alert('下载wgt失败！')
          }
          plus.nativeUI.closeWaiting()
        })
        .start()
    },
    // 获取项目用户数据
    getUserList() {
      if (!this.token || !this.projectId) {
        return
      }
      this.$store.dispatch('getUserList', this.projectId)
      this.$store.dispatch('getDepartmentList')
    },
    // 获取天气
    getWeather() {
      if (!this.token) {
        return
      }
      this.fly.system.post('weather/info', { lng: this.project.longitude, lat: this.project.latitude }).then(res => {
        let datas = res.data || {}
        this.$store.commit('setWeatherInfo', datas)
      })
    },
    // 获取日志
    getMessage() {
      if (!this.token) {
        this.list = []
        uni.stopPullDownRefresh()
        return
      }

      if (this.$appType !== 2) {
        this.queryMessageList()
      } else {
        this.getTaskTodo()
      }
    },
    // 获取我的待办
    async getTaskTodo() {
      let params = {
        sortField: 'createTime',
        sortValue: 'desc',
        taskStatus: 0,
        projectId: this.projectId || '',
        page: 1,
        rows: 10
      }
      let result = await workFlowApi.queryTaskList(params)
      let datas = result.data
      let list = datas.records || []
      list.map(item => {
        item.content = item.taskName
      })
      this.list = list
    },
    // 获取消息分类列表 @author zhangshilong
    async getCommonMessageList() {
      let result = await msgApi.getCommonMessageList()
      let datas = result.data
      let messageTypes = {}
      datas.map(item => {
        if (item.appRedirectUrl) {
          messageTypes[item.subModules] = item.appRedirectUrl
        }
      })
      this.$store.commit('setMessageType', messageTypes)
    },
    // 获取日志列表
    queryMessageList() {
      this.fly.system
        .post('common/message/list', { category: 'CONSTRCTION_MANAGE', projectId: this.projectId }, { isLoading: true })
        .then(res => {
          this.list = res.data.records || []
        })
        .catch(err => {
          this.list = []
        })
    },
    // 跳转详情
    async toDetails(item) {
      let routerUrl = this.messageTypes[item.subModules]
      if (!routerUrl) {
        this.$utils.showToast('APP无详情页，请到PC端查看!')
        return
      }
      await this.fly.system.get(`/common/message/read/${item.id}`)
      // 甲供材计划单
      if (item.categoryId === 'material_supply_plan') {
        this.$utils.toUrl(`/pages/material/planList/detail?id=${item.extension}`)
        return
      }
      // 材料确认单
      if (item.categoryId === 'material_supply_confirm_bill') {
        this.$utils.toUrl(`/pages/material/confirmation/add?ids=${item.extension}`)
        return
      }
      if (routerUrl === '/quality/statistics/details') {
        this.fly.safety.post(`/quality/acceptanceApply/page`, { projectId: this.projectId }).then(res => {
          let currentData = _.find(res.data.records, item => item.id === item.extension)
          let currentUrl = this.qualityType[currentData.acceptanceType - 1].url
          this.$utils.toUrl(`/pages/quality/statistics/acceptance/${currentUrl}/detail?id=${item.extension}`)
        })
      }
      this.$utils.toUrl(`${routerUrl}?id=${item.extension}`)
      // if (this.$appType('decoration')) {
      //   this.$utils.toUrl(`/pages/log/detail?id=${e.sourceId}&messageId=${e.messageId}`)
      // } else {
      //   let url = `${this.taskTodoPower[e.processDefinitionCategory]}?id=${e.targetId}`
      //   if (!url) {
      //     this.$utils.showToast('功能开发中，敬请期待!')
      //     return
      //   }
      //   this.$utils.toUrl(url)
      // }
    },
    // 采购通知
    procurementNotice() {
      let url = this.$hasPower('procurementPartyDetail')
        ? '/pages/material/procurementParty/index'
        : '/pages/material/procurementSupplier/index'
      this.$utils.toUrl(url)
    },
    // 跳转bim模型
    toBim() {
      this.$utils.toUrl(`/pages/bim-model/index?id=&projectId=${this.projectId}&showBtn=true&bimType=1`)
    }
  }
}
</script>

<style lang="less" scoped>
@import url('./main.less');
</style>
