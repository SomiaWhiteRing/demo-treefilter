<template>
  <div class="item">
    <div class="base">
      <el-radio-group v-model="object.rules" size="small">
        <el-radio-button label="且"></el-radio-button>
        <el-radio-button label="或"></el-radio-button>
      </el-radio-group>
    </div>
    <el-row>
      <template v-for="(item, index) in object.list">
        <el-row
          :key="'condition' + index"
          class="child"
          v-if="item.type == 'condition'"
        >
          <el-select
            v-model="item.name"
            size="small"
            style="width: 120px"
            placeholder="请选择"
          >
            <el-option v-for="(item, index) in names" :key="index" :label="item.label" :value="item.value"></el-option>
          </el-select>
          <!-- multiple  -->
          <el-select
            v-model="item.symbol"
            size="small"
            placeholder="请选择"
            style="margin-left: 10px; width: 100px"
          >
            <el-option v-for="(item, index) in symbols" :key="index" :label="item.label" :value="item.value"> </el-option>
          </el-select>
          <el-input
            v-model="item.rules"
            size="small"
            style="width: 200px; margin: 0 10px"
            placeholder="请输入规则"
          ></el-input>
          <el-button
            size="small"
            type="primary"
            icon="el-icon-share"
            circle
            @click="addBranch(index)"
            />
          <el-button
            size="small"
            type="primary"
            icon="el-icon-plus"
            circle
            @click="addChild(index)"
            />
          <el-button
            v-show="filterIndex !== -1 || object.list.filter(item => item.type == 'condition').length !== 1"
            size="small"
            icon="el-icon-minus"
            circle
            @click="deleteChild(index)"
          />
        </el-row>
        <el-row
          v-if="item.type == 'group' && item.list.length > 0"
          :key="'group' + index"
          class="child"
        >
          <TreeFilter :object="item" :filterIndex="index" @deleteItem="deleteItem" />
        </el-row>
      </template>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'TreeFilter',
  props: {
    object: {
      type: Object,
      default () {
        return {
          rules: '且',
          list: [{ type: 'condition', name: '', symbol: '', rules: '' }]
        }
      }
    },
    filterIndex: {
      type: Number,
      default: -1
    },
    names: {
      type: Array,
      default () {
        return [
          { label: '名称', value: '名称' },
          { label: '地区', value: '地区' },
          { label: 'ID', value: 'ID' }
        ]
      }
    },
    symbols: {
      type: Array,
      default () {
        return [
          { label: '等于', value: '==' },
          { label: '大于', value: '>' },
          { label: '小于', value: '<' },
          { label: '大于等于', value: '>=' },
          { label: '小于等于', value: '<=' },
          { label: '不等于', value: '!=' }
        ]
      }
    }
  },
  methods: {
    deleteChild (index) {
      this.object.list.splice(index, 1)
      if (this.object.list.length === 0) {
        // 让父组件移除该组件
        this.$emit('deleteItem', this.filterIndex)
      }
    },
    deleteItem (filterIndex) {
      this.object.list.splice(filterIndex, 1)
    },
    addChild (index) {
      this.object.list.splice(index + 1, 0, {
        name: '',
        symbol: '',
        rules: '',
        type: 'condition'
      })
    },
    addBranch (index) {
      this.object.list.splice(index + 1, 0, {
        rules: '且',
        type: 'group',
        list: [{ name: '', symbol: '', rules: '', type: 'condition' }]
      })
    }
  }
}
</script>

<style scoped lang="scss">
.item{
  display: flex;
  align-items: center;
  .base {
    position: relative;
    margin-right: 10px;
    /* 设置子元素的横线，定位在高度的50% */
    &::after {
      top: 50%;
      right: -10px;
      position: absolute;
      content: "";
      display: block;
      width: 10px;
      height: 1px;
      border: 1px solid #d9d9d9;
      border-style: solid none none none;
    }
    /* 禁止radiobox换行 */
    & > .el-radio-group {
      display: flex;
      flex-wrap: nowrap;
    }
  }
  /* 只需要左边边框线 */
  .child {
    width: 100%;
    position: relative;
    border: 1px solid #d9d9d9;
    border-style: none none none solid;
    padding: 10px 0;
    padding-left: 12px;
    display: flex;
    /* 设置一个伪元素宽2px 高50% 用于遮挡多余的左边框线 */
    &::before {
      display: block;
      content: "";
      position: absolute;
      background-color: white;
      width: 1px;
      height: 50%;
    }
    /* 设置第一个子元素的伪类定位 */
    &:first-child::before {
      left: -1px;
      top: 0;
    }
    /* 设置最后一个个子元素的伪类定位 */
    &:last-child::before {
      left: -1px;
      bottom: 0;
    }
    /* 如果只有一个子元素，则伪元素的高度翻倍 */
    &:only-child::before {
      height: 100%;
    }
    /* 设置子元素的横线，定位在高度的50% */
    &::after {
      top: 50%;
      left: 0;
      position: absolute;
      content: "";
      display: block;
      width: 10px;
      height: 1px;
      border: 1px solid #d9d9d9;
      border-style: solid none none none;
    }
    /* 如果只有一个子元素，则不显示横线 */
    &:only-child{
      padding-left: 0;
      &::after {
        display: none;
      }
    }
  }
}
</style>
