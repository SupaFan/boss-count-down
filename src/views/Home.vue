<template>
  <div class="home">
    <div class="menu">
      <van-checkbox v-model="checked" shape="square">显示已禁用</van-checkbox>
      <van-button icon="add-o"
      size="small" color="#07c160"
      @click="showAdd=true">新增倒计时</van-button>
    </div>
    <div class="list-container">
      <countDown
        v-for="(item,index) in list"
        :key="item.name+item.map"
        :name="item.name"
        :map="item.map"
        :startTime="item.startTime"
        :index="index"
        :endTime="item.endTime"
        :status="item.status"
        v-show="item.status || checked"
        @on-replay="replay(index)"
        @on-disable="handleDisable(index)"
        @on-reduction="reduction(index)"
      />
    </div>

    <van-dialog v-model:show="showAdd"
      title="新增倒计时"
      :before-close="onBeforeAdd"
      show-cancel-button>
      <van-form ref="addForm">
        <van-field
          v-model="addForm.name"
          name="名称"
          label="名称"
          placeholder="名称"
          :rules="[{ required: true, message: '请填写名称' }]"
        />
        <div class="flex-line">
          <van-cell is-link title="地图" :value="addForm.mapValue" @click="showActions = true" />
          <van-button
            icon="add-o"
            size="small"
            color="#07c160"
            @click="handleShowAddMap" />
        </div>
        <van-field
          v-model="addForm.refreshTime"
          name="刷新间隔"
          label="刷新间隔"
          placeholder="刷新间隔"
          :rules="[{ required: true, message: '请填写刷新间隔' }]"
        />
      </van-form>
    </van-dialog>
    <van-action-sheet v-model:show="showActions" :actions="mapList" @select="onSelect" />
    <!-- 新增map -->
     <van-dialog v-model:show="showAddMap" title="新增地图"
      show-cancel-button  :before-close="onBeforeClose" >
      <van-form ref="myform">
        <van-field
          v-model="mapName"
          name="地图名"
          label="地图名"
          placeholder="地图名"
          :rules="[{ required: true, message: '请填写地图名' }]"
        />
        </van-form>
    </van-dialog>
  </div>
</template>

<script>
// @ is an alias to /src
import countDown from '../components/countDown.vue';

const addForm = {
  name: '',
  mapValue: '',
  refreshTime: '',
};
export default {
  name: 'Home',
  data() {
    return {
      checked: false,
      showAdd: false,
      showAddMap: false,
      showActions: false,
      addForm: { ...addForm },
      mapName: '',
      list: [],
      mapList: [],
    };
  },
  mounted() {
    this.mapList = JSON.parse(localStorage.getItem('mapList') || '[]');
    this.list = JSON.parse(localStorage.getItem('list') || '[]');
  },
  components: {
    countDown,
  },
  methods: {
    handleShowAddMap() {
      this.showAddMap = true;
    },
    onSelect(value) {
      this.addForm.mapValue = value.name;
      this.showActions = false;
    },
    onBeforeAdd(action) {
      return new Promise((resolve) => {
        if (action === 'confirm') {
          resolve(false);
          this.$refs.addForm.validate().then(() => {
            this.handleAddBoss();
          });
        } else {
        // 拦截取消操作
          resolve(true);
        }
      });
    },
    onBeforeClose(action) {
      return new Promise((resolve) => {
        if (action === 'confirm') {
          resolve(false);
          this.$refs.myform.validate().then(() => {
            this.handleAddMap();
            this.showAddMap = false;
          });
        } else {
        // 拦截取消操作
          resolve(true);
        }
      });
    },
    // 新增boss
    handleAddBoss() {
      if (!this.addForm.mapValue) return;
      this.list.push({
        startTime: new Date(),
        endTime: this.addForm.refreshTime,
        name: this.addForm.name,
        map: this.addForm.mapValue,
        status: true,
      });
      localStorage.setItem('list', JSON.stringify(this.list));
      this.showAdd = false;
      this.addForm = { ...addForm };
    },
    // 新增地图
    handleAddMap() {
      if (!this.mapName) return;
      this.mapList.push({ name: this.mapName });
      this.mapName = '';
      localStorage.setItem('mapList', JSON.stringify(this.mapList));
    },
    // 重置刷新时间
    replay(index) {
      this.list[index].startTime = new Date();
      localStorage.setItem('list', JSON.stringify(this.list));
    },
    handleDisable(index) {
      this.list[index].status = false;
      localStorage.setItem('list', JSON.stringify(this.list));
    },
    reduction(index) {
      this.list[index].status = true;
      localStorage.setItem('list', JSON.stringify(this.list));
    },
  },
};
</script>

<style scoped lang="less">
  .home{
    padding: 20px;
    .menu {display: flex; justify-content: space-between; padding:  10px;}
    .list-container {overflow: hidden;}
    .flex-line {display: flex; align-items: center; padding-right:15px}
  }
</style>
