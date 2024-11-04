<template>
    <div class="index_page">
        <Dialog
          v-model:visible="showDialog"
          :footerBtns="footerBtns"
        >
          <template #main>123</template>
        </Dialog>
        <el-button @click="showDialog= true">打开对话框</el-button>
    </div>
</template>

<script setup>
import Dialog from './components/Dialog.vue'
import { ref } from 'vue'

const showDialog = ref(false)
const footerBtns = ref([
  {name:'取消',width:'112',function:closeDialog},
  {name:'确认',type:'primary',width:'112',function:submit},
  {name:'添加项目',type:'primary',width:'88',height:'29',function:submit},
])
function closeDialog(){
  showDialog.value = false
}
function submit(){
  ElMessage.success('操作成功!')
}
</script>

<style scoped lang="scss">
.index_page{
  width: 80%;
}
</style>
