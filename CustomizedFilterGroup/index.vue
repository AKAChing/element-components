<template>
  <div class="customizedFilterGroupContainer clx">
    <div
      class="unit"
      v-for="(value,key,index) in queryObj"
      :key="index">
      <span class="label">{{value.label}}</span>
      <el-input
        v-if="value.type == 'input'"
        v-model="state[key]"
        size="mini"
        clearable
        filterable
        suffix-icon="el-icon-search"
        :placeholder="`请输入${value.label}`">
      </el-input>
      <el-select
        v-if="value.type == 'select'"
        v-model="state[key]"
        size="mini"
        clearable
        filterable 
        :placeholder="`请选择${value.label}`">
        <el-option
          v-for="(option,index2) in value.options"
          :key="index2"
          :label="option[value.optionKey ? value.optionKey : 'label']"
          :value="option[value.optionValue ? value.optionValue : 'value']">
        </el-option>
      </el-select>
      <el-date-picker
        v-if="value.type == 'dateRangePicker'"
        v-model="dateRange[key]"
        size="mini"
        clearable
        type="daterange"
        @change="(timeRange,source) => dateChange(timeRange,value)"
        align="center"
        unlink-panels
        value-format="yyyy-MM-dd"
        range-separator="-"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        :picker-options="pickerOptions">
      </el-date-picker>
    </div>
    <div class="inlineTriggerBtn">
      <el-button class="query" type="primary" size="mini" @click="query">查 询</el-button>
      <el-button class="clear" type="info" size="mini" @click="clear">清 除</el-button>
    </div>
  </div>
</template>

<script>
export default{
  name: 'customized-filter-group',
  data(){
    return {
      pickerOptions: {
        shortcuts: [{
          text: '今天',
          onClick(picker) {
            const end = new Date();
            const start = new Date(new Date().toDateString());
            end.setTime(start.getTime());
            picker.$emit('pick', [start, end]);
          }
        }, {
          text: '最近一周',
          onClick(picker) {
            const end = new Date(new Date().toDateString());
            const start = new Date();
            start.setTime(end.getTime() - 3600 * 1000 * 24 * 7);
            picker.$emit('pick', [start, end]);
          }
        }, {
          text: '最近一个月',
          onClick(picker) {
            const end = new Date(new Date().toDateString());
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
            picker.$emit('pick', [start, end]);
          }
        }, {
          text: '最近三个月',
          onClick(picker) {
            const end = new Date(new Date().toDateString());
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
            picker.$emit('pick', [start, end]);
          }
        }]
      },
      dateRange: {},
      state: {}// 当前filterGroup的数据
    }
  },
  props: {
    queryObj: {
      type: Object
    }
  },
  methods:{
    query(){
      this.$emit('get-data',this.state)
    },
    clear(){
      Object.keys(this.state).forEach(key => this.state[key] = '')
      this.dateRange = {}
    },
    dateChange(dateRange,value){
      if(dateRange){
        this.state[value.startTime] = dateRange[0]
        this.state[value.endTime] = dateRange[1]
      }else{
        this.state[value.startTime] = ''
        this.state[value.endTime] = ''
      }
    },
    instruction(){
      // 组件说明:
      const queryObj = {
        // 每一项必须包含一个label属性和一个type属性,type属性用来指明当前项是什么类型的输入框,根据type属性的不同部分选项结构略有不同,如下
        // 1.input类型无额外属性
        // 2.select类型额外需要一个options属性,为下拉框选项数组,其中每项结构默认为{label:'名称',value: '值'},可以通过设置optionKey和optionKeyValue字段来自定义字段
        // 3.dateRangePicker类型为时间范围 所以有起止时间 通过startTime和endTime自定义字段
        name: {
          label: '用户名字',
          type: 'input',
        },
        mobile: {
          label: '手机号码',
          type: 'input',
        },
        sex: {
          label: '用户性别',
          type: 'select',
          options: [
            {label: '男', value: 0},
            {label: '女', value: 1},
          ]
        },
        sex: {
          label: '用户性别',
          type: 'select',
          options: [
            {name: '男', key: 0},
            {name: '女', key: 1},
          ],
          optionKey: 'name',
          optionValue: 'key'
        },
        onlineState: {
          label: '产品名称',
          type: 'select',
          options: [],
          optionKey: 'name',
          optionValue: 'uuid'
        },
        dateRange: {
          label: '注册时间',
          type: 'dateRangePicker',
          startTime: 'register_start_time',
          endTime: 'register_end_time'
        },
        loginRange: {
          label: '登陆时间',
          type: 'dateRangePicker',
          startTime: 'login_start_time',
          endTime: 'login_end_time'
        }
      }
    }
  }
}
</script>
<style lang="scss" scoped>
  .customizedFilterGroupContainer{
    .unit{
      padding: 4px;
      height: 62px;
      float: left;
      .label{
        color: #666;
        font-size: 13px;
        display: block;
        margin: 6px 0;
      }
      /deep/.el-input{
        display: block;
      }
      .el-date-editor{
        width: 210px;
        padding: 3px 0;
        overflow: hidden;
        /deep/.el-range-input{
          width: 50%;
        }
        /deep/.el-icon-date{
          margin-left: -20px;
        }
        /deep/.el-range__close-icon{
          position: absolute;
          right: 0;
        }
        /deep/.el-range-separator{
          width: 16px;
          padding: 0;
        }
      }
    }
    .inlineTriggerBtn{
      float: left;
      position: relative;
      height: 62px;
      .el-button{
        position: absolute;
        bottom: 4px;
      }
      .query{ 
        left: 4px;
      }
      .clear{
        left: 62px;
      }
    }
  }
</style>