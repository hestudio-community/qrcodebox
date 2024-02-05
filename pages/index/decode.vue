<template>
  <view>
    <button hover-class="button-hover" type="primary" class="button" @click="ClickScan">
      扫一扫
    </button>
    <view class="box" v-show="showresult">
      <text class="text">原始内容</text>
      <uni-easyinput v-model="result" disabled type="textarea" autoHeight />
      <button class="button" hover-class="button-hover" @click="CopyResult">
        复制全部内容
      </button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      result: '',
      showresult: false,
    }
  },
  methods: {
    ClickScan() {
      uni.scanCode({
        success: (res) => {
          this.result = res.result
          if (this.result != '') {
            this.showresult = true
          } else {
            // #ifdef APP-PLUS
            plus.nativeUI.toast('内容空！')
            // #endif

            // #ifdef MP-ALIPAY
            my.showToast({
              content: '内容空！',
              type: "error"
            })
            // #endif
			
			// #ifdef MP-WEIXIN
			wx.showToast({
			  title: '内容空！',
			  icon: "error"
			})
			// #endif
          }
        }
      })
    },
    CopyResult() {
      uni.setClipboardData({
        data: this.result,
        success: () => {
          // #ifdef APP-PLUS
          plus.nativeUI.toast('复制成功')
          // #endif

          // #ifdef MP-ALIPAY
          my.showToast({
            content: '复制成功',
            type: "success"
          })
          // #endif
		  
		  // #ifdef MP-WEIXIN
		  wx.showToast({
		    title: '复制成功',
		    icon: "success"
		  })
		  // #endif
        }
      })
    }
  },
  mounted() {
    // #ifdef MP-ALIPAY
    my.setNavigationBar({
      frontColor: '#000000',
      backgroundColor: '#ffffff',
    })
    // #endif
  }
}
</script>

<style>
.button {
  margin-top: 10px;
  margin-left: 15px;
  margin-right: 15px;
}

.box {
  margin: 15px;
  height: 100%;
}

.text {
  margin: 5px;
}

::v-deep .is-disabled {
  color: black !important;
}
</style>