<script setup lang="ts">
import type { UploadCustomRequestOptions, UploadInst } from "naive-ui";
import { NP, NIcon, NText, NUpload, NUploadDragger, useMessage } from "naive-ui";
import { ArchiveOutline } from "@vicons/ionicons5";
import { ref } from "vue";

const message = useMessage();
const uploadRef = ref<UploadInst | null>(null);

const emits = defineEmits<{
  upload: [base64: string];
}>();

function fileToBase64(file: File) {
  return new Promise<string>((resolve, reject) => {
    const reader = new FileReader();

    reader.onload = () => resolve(String(reader.result));
    reader.onerror = () => reject(reader.error ?? new Error("文件解析失败，请检查文件"));

    reader.readAsDataURL(file);
  });
}

async function customRequest({ file, onFinish, onError }: UploadCustomRequestOptions) {
  try {
    if (!(file.file instanceof File)) {
      throw new Error("文件解析失败，请检查文件");
    }

    if (!file.file.type.startsWith("image/")) {
      throw new Error("请上传图片文件");
    }

    const base64 = await fileToBase64(file.file);
    emits("upload", base64);

    onFinish();
  } catch (error) {
    uploadRef.value?.clear();
    message.error(error instanceof Error ? error.message : "上传失败，请重试");
    onError();
  }
}

function handleReset() {
  uploadRef.value?.clear();
}

defineExpose({ handleReset });
</script>

<template>
  <div class="upload-shell">
    <n-upload
      ref="uploadRef"
      multiple
      directory-dnd
      action="#"
      :max="1"
      :custom-request="customRequest"
    >
      <n-upload-dragger>
        <div style="margin-bottom: 12px">
          <n-icon size="48" :depth="3">
            <ArchiveOutline />
          </n-icon>
        </div>
        <n-text style="font-size: 16px">点击或拖动文件到这里上传</n-text>
        <n-p depth="3" style="margin: 8px 0 0 0">上传前请确认文件格式为图片</n-p>
      </n-upload-dragger>
    </n-upload>
  </div>
</template>

<style scoped>
.upload-shell {
  width: min(100%, 512px);
}
</style>
