<template>
  <div class="flex flex-col gap-4 my-[30px] md:px-5 text-[14px]">
    <div class="flex items-center">
      <div class="w-[100px]">{{ $t('repo.set.label') }}</div>
      <el-input
        v-model="label"
        :maxLength="200"
        show-word-limit
        clearable
        class="w-[380px] h-[40px] text-gray-500 ml-4"
      />
    </div>
    <div class="flex items-center">
      <div class="w-[100px]">{{ $t('repo.set.path') }}</div>
      <el-input
        v-model="path"
        :maxLength="200"
        show-word-limit
        clearable
        class="w-[380px] h-[40px] text-gray-500 ml-4"
      />
    </div>
    <div class="mt-4">
      <el-button
        type="primary"
        @click="submit"
        >{{ $t('repo.set.submit') }}</el-button
      >
      <el-button @click="cancel">{{ $t('repo.set.cancel') }}</el-button>
    </div>
  </div>
</template>
<script setup>
  import { ref, onMounted } from 'vue'
  import useFetchApi from '../../packs/useFetchApi'
  import { ElMessage } from 'element-plus'

  const props = defineProps({
    repoName: String,
    namespacePath: String,
    currentPath: String,
    currentBranch: String
  })

  const prefixPath = document.location.pathname.split('/')[1]

  const label = ref('')
  const path = ref('')

const getSet = async() => {
    try {
      const { data, error } = await useFetchApi(
        `/${prefixPath}/${props.namespacePath}/clabel/${props.currentPath}?ref=${props.currentBranch}`
      ).json()

      console.log('getres', data, error)

      if (!error.value) {
        const result = data.value
        label.value = result?.data?.label || ''
        path.value = result?.data?.['annotation_path'] || ''
      } else {
        ElMessage({ message: error.value.msg, type: 'error' })
      }
    } catch (err) {
      ElMessage({ message: err.message, type: 'error' })
    }
  }

  const redirectToFilePreview = () => {
    window.location.href = `/${prefixPath}/${props.namespacePath}/blob/${props.currentBranch}/${props.currentPath}`
  }

  const cancel = () => {
    redirectToFilePreview()
  }

  const submit = async () => {
    const params = {
      label: label.value,
      annotation_path: path.value,
    }
    const options = {
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(params)
    }
    try {
      const { data, error } = await useFetchApi(
        `/${prefixPath}/${props.namespacePath}/clabel/${props.currentPath}?ref=${props.currentBranch}`, options
      ).post().json()

      console.log('postres', data, error)

      if (!error.value) {
        ElMessage({ message: '修改成功', type: 'success' })
        redirectToFilePreview()
      } else {
        ElMessage({ message: error.value.msg, type: 'error' })
      }
    } catch (err) {
      ElMessage({ message: err.message, type: 'error' })
    }
  }

  onMounted(() => {
    getSet()
  })
</script>

<style scoped>
  :deep(.el-radio__label) {
    color: #303133 !important;
    font-weight: 400;
  }
</style>
