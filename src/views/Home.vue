<template>
  <div class="home">
    <el-card class="box-card" style="margin-top: 10px">
      <TreeFilter :object="filter" />
    </el-card>
    <el-card class="box-card" style="text-align: left">
      <pre>{{ filter }}</pre>
    </el-card>
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
        rules: '且',
        list: [{ type: 'condition', name: '', symbol: '', rules: '' }]
      }
    }
  }
}
</script>
