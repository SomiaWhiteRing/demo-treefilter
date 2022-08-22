<template>
  <div class="home">
    <el-card class="box-card" style="margin-top: 10px">
      <TreeFilter :object="filter" />
    </el-card>
    <el-tabs type="border-card">
      <el-tab-pane label="源数据视图">
          <pre style="text-align: left">{{ filter }}</pre>
      </el-tab-pane>
      <el-tab-pane label="程序语言视图">
        <pre style="text-align: left">{{ destructor(filter) }}</pre>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import TreeFilter from '@/components/TreeFilter.vue'

export default {
  name: 'Home',
  components: {
    TreeFilter
  },
  data () {
    return {
      filter: {
        relation: '&&',
        relationShow: '且',
        list: [{ type: 'condition', name: '', symbol: '', rule: '' }]
      }
    }
  },
  methods: {
    destructor (object, step = false) {
      let result = ''
      object.list.map((item, index) => {
        if (item.type === 'condition') {
          result += ` ${item.name}${item.name ? ' ' : ''}${item.symbol}${item.symbol ? ' ' : ''}${item.rule}${item.rule ? ' ' : ''}${object.relation}`
        } else {
          result += ` ${this.destructor(item, true)}${object.relation}`
        }
      })
      if (step) {
        return '(' + result.substring(0, result.length - object.relation.length) + ') '
      }
      return result.substring(0, result.length - object.relation.length)
    }
  }
}
</script>
