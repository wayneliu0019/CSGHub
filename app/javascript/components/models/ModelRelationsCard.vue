<template>
  <div class="p-[16px] border-t border-[#EBEEF5]">
    <h3 class="flex items-center gap-[8px]">
      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18" fill="none">
        <path opacity="0.12" d="M9 16.5C12.7279 16.5 15.75 15.4926 15.75 14.25V3.75C15.75 3.75 15.375 6 9 6C2.625 6 2.25 3.75 2.25 3.75V14.25C2.25 15.4926 5.27208 16.5 9 16.5Z" fill="#1F75CB"/>
        <path d="M15.75 9C15.75 10.2426 12.7279 11.25 9 11.25C5.27208 11.25 2.25 10.2426 2.25 9M15.75 3.75C15.75 4.99264 12.7279 6 9 6C5.27208 6 2.25 4.99264 2.25 3.75M15.75 3.75C15.75 2.50736 12.7279 1.5 9 1.5C5.27208 1.5 2.25 2.50736 2.25 3.75M15.75 3.75V14.25C15.75 15.4926 12.7279 16.5 9 16.5C5.27208 16.5 2.25 15.4926 2.25 14.25V3.75" stroke="#1F75CB" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <span class="font-[500] text-[#344054] text-[16px] leading-[24px]"> 关联模型 </span>
      <span class="text-[#344054]"> {{ namespacePath }} </span>
    </h3>
    <div class="mt-[16px] flex flex-col gap-[12px]">
      <RepoItem v-for="model in models" :repo="model" repoType="model" cardType="relations" />
    </div>
  </div>
</template>

<script setup>
  import RepoItem from '../shared/RepoItem.vue';

  const props = defineProps({
    namespacePath: String,
    models: Array
  })
</script>