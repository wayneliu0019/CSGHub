<template>
  <div class="flex flex-col gap-4 my-[30px] md:px-5">
    <div class="flex items-center gap-[10px]">
      <div class="whitespace-nowrap">{{ repoName }}</div>
      <div class="text-[#909399]"> / </div>
      <el-input v-model="fileName"
                :maxLength="200"
                show-word-limit
                clearable
                placeholder="文件名"
                @input="handleFileNameChange"
                class="w-full h-[40px] text-[#606266]" />
    </div>
    <code-editor v-model="codeContent" />
    <el-radio-group v-model="new_branch" class="my-4 py-4 border border-[#DCDFE6] border-l-0 border-r-0">
      <el-radio label="main" size="large">直接提交到 main 分支</el-radio>
    </el-radio-group>
    <div>
      <div class="mb-2 text-sm">提交新文件</div>
      <el-input v-model="commitTitle"
                :maxLength="200"
                show-word-limit
                clearable
                :placeholder="commitTitlePlaceholder"
                class="w-full h-[40px] text-[#606266]" />
    </div>
    <CommunityMDTextarea desc="" placeholder="提供更多描述" @inputChange="handleCommentInputChange"></CommunityMDTextarea>
    <div>
      <el-button type="primary" @click="createFile" :disabled="!commitValid || submiting">创建文件</el-button>
      <el-button @click="cancel">取消</el-button>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import CodeEditor from '../shared/CodeEditor.vue'
import CommunityMDTextarea from '../community/CommunityMDTextarea.vue'
import csrfFetch from "../../packs/csrfFetch"

const props = defineProps({
  originalCodeContent: String,
  repoName: String,
  namespacePath: String
})

const codeContent = ref(props.originalCodeContent)
const commitTitle = ref('')
const commitDesc = ref('')
const fileName = ref('')
const new_branch = ref('main')
const commitTitlePlaceholder = ref('Create new file')
const commitValid = ref(false)
const submiting = ref(false)

const prefixPath = document.location.pathname.split('/')[1]

const handleCommentInputChange = (value) => {
  commitDesc.value = value
}

const handleFileNameChange = (value) => {
  if (value.trim() === '') {
    commitTitlePlaceholder.value = `Create new file`
    commitValid.value = false
  } else {
    commitTitlePlaceholder.value = `Create ${value}`
    commitValid.value = true
  }
}

const createFile = async () => {
  submiting.value = true
  // TODO: main branch for now; should support different branches
  const createFileEndpoint = `/internal_api/${prefixPath}/${props.namespacePath}/files/main`
  const bodyData = {
    path: fileName.value,
    content: codeContent.value,
    commit_title: commitTitle.value,
    commit_desc: commitDesc.value
  }
  const option = {
    method:'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(bodyData)
  }
  const response = await csrfFetch(createFileEndpoint, option)
  if (response.ok) {
    redirectToFilePreview()
  } else {
    return response.json().then(data => { throw new Error(data.message) }).finally(() => {
      submiting.value = false
    })
  }
}

const redirectToFilePreview = () => {
  window.location.href = `/${prefixPath}/${props.namespacePath}/blob/main/${fileName.value}`
}

const cancel = () => {
  window.location.href = `/${prefixPath}/${props.namespacePath}/files/main`
}
</script>

<style scoped>
  :deep(.el-radio__label) {
    color: #303133 !important;
    font-weight: 400;
  }
</style>