<template>
  <div class="about">
    <h2>简单的倒计时辅助</h2>
    <h3>计划更新</h3>
    <p>1. 桌面通知功能 ✅</p>
    <p>2. 手机震动提示 ✅</p>
    <p>2. 智能语音提示 </p>
    <p>3. 排序功能： 按地图排序 、按刷新时间排序</p>

    <h3>内置数据</h3>
    <p>内置常见地图、boss</p>
    <van-button icon="back-top"   color="#07c160"
          size="small" style="margin-right:5px" @click="set">内置数据</van-button>
          <p>点击此按钮会清除你以前设置的数据</p>

    <h3>其他</h3>
    <p>联系作者 VX: remkisss</p>
  </div>
</template>
<script>
import { Toast } from 'vant';

export default {
  name: 'about',
  data() {
    return {
      maps: [],
      mapList: [
        { name: '尸王殿' },
        { name: '猪洞一' },
        { name: '猪洞二' },
        { name: '猪洞三' },
        { name: '猪洞四' },
        { name: '猪洞五' },
        { name: '猪洞七' },
        { name: '丽影古墓' },
        { name: '世外桃源' },
        { name: '未知暗殿' },
        { name: '蜈蚣洞' },
        { name: '封魔谷' },
      ],
      list: [
        {
          endTime: '20',
          map: '猪洞一',
          name: '野猪王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞一',
          name: '邪恶蛇蝎',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞二',
          name: '野猪王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞二',
          name: '邪恶蛇蝎',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞三',
          name: '野猪王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞三',
          name: '邪恶蛇蝎',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞四',
          name: '野猪王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞四',
          name: '邪恶蛇蝎',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞五',
          name: '野猪王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞五',
          name: '邪恶蛇蝎',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞七',
          name: '野猪王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '猪洞七',
          name: '邪恶蛇蝎',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '15',
          map: '丽影古墓',
          name: '骷髅王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '20',
          map: '尸王殿',
          name: '骷髅王',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '60',
          map: '未知暗殿',
          name: '鸡',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '30',
          map: '封魔谷',
          name: '封魔教主',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '30',
          map: '世外桃源',
          name: '祖玛教主一',
          startTime: new Date(),
          status: true,
        },
        {
          endTime: '30',
          map: '世外桃源',
          name: '祖玛教主二',
          startTime: new Date(),
          status: true,
        },
      ],
    };
  },
  methods: {
    set() {
      localStorage.setItem('mapList', JSON.stringify(this.mapList));
      localStorage.setItem('list', JSON.stringify(this.list));
      Toast.success('内置成功');
    },
  },
};
</script>

<style lang="less" scoped>
  .about {
    padding: 20px;
  }
</style>
