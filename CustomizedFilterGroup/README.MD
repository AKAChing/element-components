# 自定义搜索框组件

> 引用并注册组件

``` javascript
  import { CustomizedFilterGroup } from './index.vue'
```

``` html
  <customized-filter-group
    :query-obj="queryObj"
    @get-data="getData">
  </customized-filter-group>
```

``` javascript
  let queryObj = {
    mobile: {
      label: '手机号码',
      type: 'input'
    },
    disabled: {
      label: '启用状态',
      type: 'select',
      options: [{label: '启用', value: true},{label: '禁用', value: false}]
    },
    date_range: { // date_range 此项可以随便填, 最终返回的是下面的start_time和end_time两个字段
      label: '创建时间',
      type: 'datePicker',
      startTime: 'start_time',
      endTime: 'end_time',
    }
  }

  getData(obj){
    // obj对象会返回如下数据格式
    // {
    //  mobile: '17777777777',
    //  disabled: true,
    //  start_time: '2019-5-20',
    //  end_time: '2019-5-20',
    // }
  }
```

> 参数

| 参数 | 说明 | 类型 | 默认值 |
| :--: | :--: | :--: | :--: |
| query-obj | 筛选对象 | object | - |

此组件通过传入一个query-obj对象, 并通过配置该对象的每个属性来生成对应的输入/选择/日期筛选框

queryObj[key] => 为最终输出的过滤字段, 比如queryObj.name最终返回的对象里就会包括name这个字段

queryObj[key].label => 为此项的label信息, 描述当前项是过滤哪种信息的

queryObj[key].type => 有三种类型 分别是 input/select/dateRangePicker
  - input 会渲染为一个输入框, 输入什么值就get-data的时候就会返回什么值
  - select 会渲染为一个下拉单选框, 点击某个选项get-data的时候就会返回对应的值
  - dateRangePicker 会渲染为一个YMD时间范围选择器, 选择起止时间get-data的时候就会返回对应的时间范围


queryObj[key].type === 'input'
- 没有额外属性

queryObj[key].type === 'select'
- queryObj[key].options 是一个数组, 一个展示下拉数据的对象数组, 每个对象默认结构为: { label: '', value: '' } 
- queryObj[key].optionKey/queryObj[key].optionValue 自定义options数组的key/value, 比如optionKey: 'name',optionValue: 'key', 则options数组里的对象结构会变为: { name: '', key: '' }, 当options不确定时或者需要传入一个自定义的options数组, 而options数组的键值对并不是label/value时可以通过这两项自定义对应的属性

queryObj[key].type === 'dateRangePicker'
- queryObj[key].startTime 开始时间
- queryObj[key].endTime 结束时间

> 事件

| 方法名 | 说明 | 参数 |
| :--: | :--: | :--: |
| get-data | 返回筛选数据 | obj |
| clear | 清除筛选数据 | - |
