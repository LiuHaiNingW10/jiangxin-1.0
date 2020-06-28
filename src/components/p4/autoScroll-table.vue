<template>
    <div>
        
    </div>
</template>
<script>
export default {
  name: "tableAuto",
  props: ['tableDatas', 'ids','columns'],
  data() {
    return {
      todos: [],
      tableData: this.tableDatas,
      id: this.ids,
      column: this.columns,
    };
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      console.log(this.tableData,this.id,this.column)
    }
  },

}
</script>