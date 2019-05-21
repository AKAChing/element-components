<template>
  <div class="container">
    <el-form
      :label-position="labelPosition"
      :label-width="labelWidth"
    >
      <el-form-item label="审核操作">
        <el-select v-model="stateObj[$props.approveKey]" size="mini" @change="approvalStatusChange">
          <el-option label="审核通过" :value="$props.successValue"></el-option>
          <el-option label="审核未通过" :value="$props.failureValue"></el-option>
        </el-select>
        <el-select
          v-if="stateObj[$props.approveKey] == $props.failureValue"
          v-model="stateObj[$props.reasonKey]"
          size="mini"
          allow-create
          filterable
          placeholder="请选择拒绝原因,支持自定义创建"
        >
          <el-option
            v-for="(item,index) in $props.reasonList"
            :key="index"
            :label="item"
            :value="item"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          size="mini"
          @click="trigger"
          :disabled="
            stateObj[$props.approveKey] == '' ||
            ( stateObj[$props.approveKey] == $props.failureValue && stateObj[$props.reasonKey] == '' )"
        >审核</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
/*
此组件通常配合模态框进行使用,
请在父组件引用此组件式传入v-if="此组件的父级模态框的dialogVisible",
添加此项可以清空此组件的stateObj对象,避免多次打开模态框时旧的stateObj仍然存在
*/
export default {
  name: "customized-verify",
  data() {
    return {
      stateObj: {} // 发送给父组件的数据 包括通过状态,原因(如果拒绝的话),通过时间
    };
  },
  props: {
    labelWidth: {
      // label宽度
      type: String,
      default: "100px"
    },
    labelPosition: {
      // label位置
      type: String,
      default: "left"
    },
    reasonList: {
      // 审核失败原因列表
      type: Array,
      default: () => [
        "证件照片模糊,不清晰",
        "证件号与证件照不一致",
        "真实姓名与证件照不一致",
        "其他原因"
      ]
    },
    approveKey: {
      // 审核状态key
      type: String,
      default: "approval_status"
    },
    successValue: {
      // 审核成功value
      type: String,
      default: "approved"
    },
    failureValue: {
      // 审核失败value
      type: String,
      default: "rejected"
    },
    reasonKey: {
      // 审核失败原因字段
      type: String,
      default: "reason"
    }
  },
  methods: {
    approvalStatusChange(e) {
      // 当通过状态改变时清空reason字段 防止旧的reason存在
      this.stateObj[this.$props.reasonKey] = "";
    },
    trigger() {
      this.$emit("audited", this.stateObj);
    },
    instruction() {
      // 组件说明:
      const props = {
        approveKey: "", // 表单提交时所需要的审核状态的字段名称,默认为approval_status
        successValue: "", // 审核状态的两种值之一,对应的成功值,默认为approved
        failureValue: "", // 审核状态的两种值之一,对应的失败值,默认为rejected
        reasonKey: "" // 失败原因字段,当审核状态失败时,提供的原因说明
      };
      const auditSuccess = state => {
        return state; // 组件点击审核按钮的自定义事件,state为处理完成返回出去的状态对象,包括approveKey,reasonKey
      };
    }
  },
  created() {
    // 由于vue不允许在已经创建的实例上动态添加新的根级响应式属性,加了也不是响应式的,所以在组件创建周期调用$set方法来设置相应的属性
    [
      this.$props.approveKey,
      this.$props.reasonKey
    ].forEach(item => this.$set(this.stateObj, item, ""));
  }
};
</script>