<template>
    <div :class="['list-item', {'item-success': millisecond<tipsTime*60*1000},{disable: !status}]">
      <div class="content">
        <span class="name">{{name}}</span>
        <span class="map">{{map}}</span>
        <span class="remaining">{{surplus}}</span>
        <div class="control">
          <van-button icon="replay"  plain color="#07c160"
          size="mini" v-if="status" style="margin-right:5px" @click="replay"></van-button>

          <van-button icon="closed-eye"
          color="#999" plain
          size="mini"
          v-if="status"
          @click="handleDisable"></van-button>

           <van-button icon="revoke"
           v-else
          color="#07c160" plain
          size="mini"
          @click="reduction"></van-button>

        </div>
      </div>
    </div>
</template>
<script>
import dayjs from 'dayjs';
import Push from 'push.js';
import supa from '@/assets/supa.jpeg';

export default {
  replace: true,
  data() {
    return {
      supa,
      millisecond: 0,
      percentage: 1,
      surplus: '-- : -- : --',
      tipsTime: 1.8, // 提前两分钟提示
      id: null,
      canshak: false,
      shock: true,
    };
  },
  created() {
    Push.Permission.request();
  },
  watch: {
    startTime() {
      clearInterval(this.id);
      this.render();
    },
  },
  props: {
    startTime: {},
    index: {
      type: Number,
    },
    endTime: {
      type: [Number, String],
    },
    name: {
      type: String,
    },
    map: {
      type: String,
    },
    status: {
      type: Boolean,
    },

  },
  mounted() {
    if (this.status) {
      this.render();
    }
  },
  methods: {
    // 抖动提示
    vibration() {
      navigator.vibrate = navigator.vibrate
      || navigator.webkitVibrate
      || navigator.mozVibrate
      || navigator.msVibrate;
      if (navigator.vibrate) {
        this.canshak = true;
        navigator.vibrate([500, 300, 400, 600]);
        this.shock = false;
      }
    },
    pushMessage(message) {
      Push.create('BOSS刷新提示', {
        body: message,
        requireInteraction: true,
        icon: supa,
        timeout: 3000,
        onClick() {
          this.close();
        },
      });
    },
    render() {
      // 结束时间戳
      const $endTime = dayjs(this.startTime).valueOf() + this.endTime * 60 * 1000;
      this.id = setInterval(() => {
        // 剩余时间 毫秒
        this.millisecond = dayjs($endTime).valueOf() - dayjs().valueOf();
        if (this.shock && this.millisecond < this.tipsTime * 60 * 1000) {
          this.vibration();
          this.pushMessage(`${this.map}的${this.name}还有2分钟要刷新了哟！`);
        }
        if (this.millisecond > 0) {
        // 剩余时间 HH:mm:ss
          const $h = Math.floor((this.millisecond / 1000 / 3600) % 24);
          const $m = Math.floor(this.millisecond / 1000 / 60);
          const $s = Math.floor((this.millisecond / 1000) % 60);
          this.surplus = `${this.ZeorFill($h)}:${this.ZeorFill($m)}:${this.ZeorFill($s)}`;
          // 剩余百分比
          this.percentage = (1 - this.millisecond / (this.endTime * 60 * 1000)).toFixed(2);
        } else {
          clearInterval(this.id);
          this.shock = true;
          this.surplus = '00:00:00';
        }
      }, 1000);
    },
    ZeorFill(num) {
      return num >= 10 ? num : `0${num}`;
    },
    replay() {
      this.shock = true;
      this.$emit('on-replay', this.index);
    },
    handleDisable() {
      this.$emit('on-disable', this.index);
    },
    reduction() {
      this.$emit('on-reduction', this.index);
    },
  },
};
</script>
<style lang="less" scoped>
.list-item {
      border-radius: 100px;
      padding:5px 10px ;
      margin-top: 10px;
      position: relative;
      box-sizing: border-box;overflow: hidden;
      box-shadow: 0 0 30px #f5f0f0 inset;
      background: linear-gradient(45deg,#f5f0f0, #f8f8f8);
      &.item-success {color: #07c160;}
      &.disable {color: #999;}
      .name,
      .map,
      .remaining,
      .control{width: 25%; text-align: center;}
      .percentage {
        position: absolute;
        height: calc(100% - 20px);
        background-image: linear-gradient(30deg, #38A6CA, #1d4d5e);
        left:0;top:10px;
        border-radius: 100px;
      }
      .name{text-align: left;}
      .control {text-align: right;}
      .content {
       display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 15px;
        left:0;top:0;
      }
    }

</style>
