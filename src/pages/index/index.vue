<template>
  <div class="container">

      <view class="section">
        <view class="flex-wrp">
          <view class="flex-item bc_blue" >
            <picker mode = "selector" @change="bindPickerChange" value=0 :range="array" range-key="name">
                当前选择：{{curName}} (点击切换)
            </picker>
          </view>
          <div class="flex-warp">
            <view class="flex-item bc_green"><input type="text" class="form-control input_postId" v-model="inputCon.postid" placeholder="请输入快递单号" /></view>
            <view class="flex-item bc_red"><button type="primary" size="mini" @click="getInfo()">点我查询</button></view>
          </div>
        </view>
      </view>

    <view class="section">
      <ul class="list">
        <li v-for="(item,index) in resData" :key="index">
           <span>{{item.time}}</span><span>{{item.context}}</span>
        </li>
      </ul>
    </view>
  </div>
</template>

<script>

export default {
  data () {
    return {
      inputCon: {
        postid:'213198066199',
        type:'zhongtong'
      },
      index:0,
      curType:0,
      array:[
        {
          id:'zhongtong',
          name:'中通'
        },
        {
          id:'yuantong',
          name:'圆通'
        }
      ],
      resData:[]
    }
  },
  computed:{
    curName:function() {
      return this.array[this.curType].name
    }
  },
  methods: {
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    bindPickerChange(e){
      this.curType = e.target.value
      this.inputCon.type = this.array[this.curType].id;
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    },
    getInfo(){
      const data = this.inputCon
      const _this = this;
      _this.resData = [];
      wx.request({
        url: 'https://www.kuaidi100.com/query',
        data:data,
        header: {
          'content-type': 'application/json' // 默认值
        },
        success: function(res) {
          console.log('res::',res)
          if(res.data.message=='ok'){
             _this.resData =  _this.resData.concat(res.data.data);
          }
        }
      })

    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style scoped>
  .list{
      padding:10rpx;
      margin-top:10rpx;
      font-size:12px;
  }
  .list li{
    padding-bottom:20rpx;
    border-bottom:1px solid #ccc;
    min-height:50rpx;
  }
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}
.flex-warp{
  display: flex;
}
.input_postId{
  margin-top:5px;
}
.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}

</style>
