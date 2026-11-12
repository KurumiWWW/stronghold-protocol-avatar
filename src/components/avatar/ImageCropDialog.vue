<script setup lang="ts">
import { NButton, NFlex, NModal } from "naive-ui";
import { VueCropper } from "cropper-next-vue";
import { ref } from "vue";

defineProps<{ img: string }>();
const show = defineModel<boolean>("show", { required: true });
const emit = defineEmits<{ crop: []; afterLeave: [] }>();
const cropperRef = ref<{ getCropData: () => Promise<string> } | null>(null);

defineExpose({ getCropData: () => cropperRef.value?.getCropData() });
</script>

<template>
  <n-modal v-model:show="show" :on-after-leave="() => emit('afterLeave')">
    <div class="w-[min(90vw,480px)] rounded-[20px] bg-white p-[clamp(16px,4vw,24px)]">
      <vue-cropper
        ref="cropperRef"
        class="mx-auto block aspect-square w-full"
        :img="img"
      />
      <n-flex justify="end" class="mt-3">
        <n-button type="primary" @click="emit('crop')">提交</n-button>
      </n-flex>
    </div>
  </n-modal>
</template>
