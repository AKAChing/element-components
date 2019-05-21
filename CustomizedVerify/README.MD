# 自定义审核组件

> 引用并注册组件

``` javascript
  import { CustomizedVerify } from './index.vue'
```

> [在线Demo](https://jsbin.com/vipibot/edit?html,console,output)

``` html
  <customized-verify
    v-if="parentDialogVisible"
    approve-key="state"
    success-value="yes"
    failure-value="no"
    reason-key="why"
    @audited="audited">
  </customized-verify>

```

``` javascript 
  audited(auditedObj){
    console.log(auditedObj)
    // auditedObj对象数据结构如下
    // {
    //   state: 'yes/no',
    //   why: 'one of the reason list'
    // }
  }
```

> 参数

| 参数 | 说明 | 类型 | 默认值 |
| :--: | :--: | :--: | :--: |
| labelWidth | 组件label宽度 | string | '100px' |
| labelPosition | 组件label居中位置 | string | 'left' |
| reasonList | 审核失败原因列表 | array | ['证件照片模糊,不清晰', '证件号与证件照不一致', '真实姓名与证件照不一致', '其他原因'] |
| approveKey | 审核状态字段的key | string | 'approval_status' |
| successValue | 审核成功value | string | 'approved' |
| failureValue | 审核失败value | string | 'rejected' |
| reasonKey | 审核失败reason的key | string | 'reason' |

> 事件

| 方法名 | 说明 | 参数 |
| :--: | :--: | :--: |
| audit-success | 返回审核状态数据 | auditedObj |