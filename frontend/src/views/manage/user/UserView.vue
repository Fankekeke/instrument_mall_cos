<template>
  <a-modal v-model="show" title="用户订单" @cancel="onClose" :width="1000">
    <template slot="footer">
      <a-button key="back" @click="onClose" type="danger">
        关闭
      </a-button>
    </template>
    <div style="font-size: 12px;font-family: SimHei" v-if="userData !== null">
      <div style="background:#ECECEC; padding:24px;font-size: 13px;">
        <a-row :gutter="15">
          <a-col :span="12" v-for="(item, index) in orderList" :key="index" style="margin-top: 15px">
            <a-card hoverable>
              <a-card-meta :title="item.shopName + '的店铺'" :description="item.name">
                <a-avatars
                  slot="avatar"
                  :src="'http://127.0.0.1:9527/imagesWeb/' + item.shopAvatar"
                />
              </a-card-meta>
              <div style="margin-left: 45px;margin-top: 20px">
                <a-row :gutter="8" style="font-size: 12px;font-family: SimHei">
                  <a-col :span="8">价 格：￥{{ item.orderPrice }}</a-col>
                  <a-col :span="16">下单时间：{{ item.createDate }}</a-col>
                </a-row>
              </div>
            </a-card>
          </a-col>
          <a-col v-if="orderList.length === 0">
            无订单
          </a-col>
        </a-row>
      </div>
    </div>
  </a-modal>
</template>

<script>
import moment from 'moment'
moment.locale('zh-cn')
export default {
  name: 'userView',
  props: {
    userShow: {
      type: Boolean,
      default: false
    },
    userData: {
      type: Object
    }
  },
  computed: {
    show: {
      get: function () {
        return this.userShow
      },
      set: function () {
      }
    }
  },
  data () {
    return {
      loading: false,
      fileList: [],
      previewVisible: false,
      previewImage: '',
      orderList: []
    }
  },
  watch: {
    userShow: function (value) {
      this.dataInit()
    }
  },
  methods: {
    dataInit () {
      this.$get('/cos/order-info/orderListByUserId', {userId: this.userData.id}).then((r) => {
        this.orderList = r.data.data
      })
    },
    onClose () {
      this.$emit('close')
    }
  }
}
</script>

<style scoped>
>>> .ant-card-meta-title {
  font-size: 13px;
  font-family: SimHei;
}
>>> .ant-card-meta-description {
  font-size: 12px;
  font-family: SimHei;
}
>>> .ant-divider-with-text-left {
  margin: 0;
}

>>> .ant-card-head-title {
  font-size: 13px;
  font-family: SimHei;
}
>>> .ant-card-extra {
  font-size: 13px;
  font-family: SimHei;
}
</style>
