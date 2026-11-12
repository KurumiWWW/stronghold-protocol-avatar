<script setup lang="ts">
import { NButton, NFlex, NModal } from "naive-ui";

defineProps<{ img: string }>();
const show = defineModel<boolean>("show", { required: true });
const emit = defineEmits<{ download: []; afterLeave: [] }>();
</script>

<template>
  <n-modal v-model:show="show" :on-after-leave="() => emit('afterLeave')">
    <div class="w-[min(90vw,530px)] rounded-[20px] bg-white p-[clamp(16px,4vw,24px)]">
      <img :src="img" alt="" class="block" />
      <n-flex justify="start" class="mt-3">
        <p>tips：移动端若出现导出按钮无法使用的情况，可以通过手动长按保存至本地</p>
        <n-button class="w-full" type="primary" @click="emit('download')">导出</n-button>
      </n-flex>
    </div>
  </n-modal>
</template>
