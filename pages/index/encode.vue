<template>
  <view>
    <view class="box">
      <uni-easyinput v-model="value" class="input" type="textarea" autoHeight :styles="styles" placeholder="你可以输入任何内容，包括文字，网页等。"
        @confirm="makecode" />
      <button :loading="makeloading" class="button" type="primary" hover-class="button-hover" @click="makecode">
        生成
      </button>
    </view>
    <view v-show="showmake" class="box">
      <uqrcode ref="uqrcode" canvas-id="qrcode" :value="origin" :options="{ margin: 25 }" @complete="makecallback">
      </uqrcode>
      <button :loading="saveloading" class="button" hover-class="button-hover" @click="savecode">
        保存二维码到相册
      </button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      origin: '',
      value: '',
      makeloading: false,
      saveloading: false,
      makesuccess: false,
      showmake: false,
      styles: {
        color: '#333'
      }
    }
  },
  methods: {
    makecallback(success) {
      if (success) {
        this.makesuccess = true
      } else {
        this.makesuccess = false
      }
    },
    makecode() {
      this.loading = true
      this.origin = this.value
      if (this.origin != '') {
        this.showmake = true
      } else {
        this.showmake = false
        // #ifdef APP-PLUS
        plus.nativeUI.toast('内容不能为空')
        // #endif

        // #ifdef MP-ALIPAY
        my.showToast({
          content: '内容不能为空',
          type: "error"
        })
        // #endif

        // #ifdef MP-WEIXIN
        wx.showToast({
          title: '内容不能为空',
          icon: "error"
        })
        // #endif
      }
      this.loading = false
    },
    savecode() {
      if (this.makesuccess) {
        this.$refs.uqrcode.save({
          success: () => {
            // #ifdef APP-PLUS
            plus.nativeUI.toast('保存成功')
            // #endif

            // #ifdef MP-ALIPAY
            my.showToast({
              content: '保存成功',
              type: "success"
            })
            // #endif

            // #ifdef MP-WEIXIN
            wx.showToast({
              title: '保存成功',
              icon: "success"
            })
            // #endif
          }
        })
      } else {
        // #ifdef APP-PLUS
        plus.nativeUI.toast('保存失败')
        // #endif

        // #ifdef MP-ALIPAY
        my.showToast({
          content: '保存失败',
          type: "error"
        })
        // #endif

        // #ifdef MP-WEIXIN
        wx.showToast({
          title: '保存失败',
          icon: "error"
        })
        // #endif
      }
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
  margin-bottom: 10px;
}

.box {
  margin: 15px;
  height: 100%;
}

.input {
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>