<template>
  <div class="flex min-h-screen justify-center px-4 py-[clamp(24px,4vw,24px)]">
    <div class="flex w-full max-w-[640px] flex-col items-center">
      <p class="m-0 text-center text-[rgb(14_116_144)] [font-size:clamp(2rem,4vw,2.25rem)] font-bold">
        卫戍协议头像生成器
      </p>
      <p class="m-0 text-center text-[rgb(156_163_175)] [font-size:clamp(1rem,2.5vw,1.25rem)]">
        卫吗？
      </p>
      <image-upload @upload="handleUpload" ref="uploadRef" />
      <avatar-controls
        v-model:show-badge="customForm.showBadge"
        v-model:badge-style="customForm.badgeStyle"
        v-model:badge-size="customForm.badgeSize"
        v-model:offset-x="customForm.offsetX"
        v-model:offset-y="customForm.offsetY"
        v-model:filter="customForm.filter"
        @reset-position="resetPosition"
      />
      <avatar-preview :img="cropImageBase64" :config="customForm" />
      <div class="mt-[10px] flex w-full max-w-[512px] flex-wrap justify-end gap-3">
        <n-button class="max-[640px]:flex-[1_1_160px]" @click="handleReset">重置</n-button>
        <n-button
          class="max-[640px]:flex-[1_1_160px]"
          type="primary"
          @click="handleImage"
          :disabled="!cropImageBase64"
          >生成
        </n-button>
      </div>
      <div class="mt-[10px]">
        <n-button quaternary round @click="toLink">
          <template #icon>
            <n-icon size="20">
              <LogoGithub />
            </n-icon>
          </template>
          <span class="text-[16px]">GitHub</span>
        </n-button>
        <n-button quaternary round @click="toBili">
          <template #icon>
            <n-icon size="20">
              <icon-bili />
            </n-icon>
          </template>
          <span class="text-[16px]">Bilibili</span>
        </n-button>
      </div>
    </div>
  </div>
  <image-crop-dialog
    ref="cropperRef"
    v-model:show="showModal"
    :img="imageBase64"
    @crop="handleCrop"
    @after-leave="onAfterLeave"
  />
  <export-result-dialog
    v-model:show="showResModal"
    :img="urls"
    @download="handleDownload"
    @after-leave="onAfterLeaveRes"
  />
</template>

<script setup lang="ts">
import html2canvas from "html2canvas";
import { NButton, NIcon } from "naive-ui";
import { reactive, ref } from "vue";
import { LogoGithub } from "@vicons/ionicons5";
import AvatarControls from "@/components/avatar/AvatarControls.vue";
import AvatarPreview from "@/components/avatar/AvatarPreview.vue";
import ExportResultDialog from "@/components/avatar/ExportResultDialog.vue";
import ImageCropDialog from "@/components/avatar/ImageCropDialog.vue";
import ImageUpload from "@/components/avatar/ImageUpload.vue";
import IconBili from "@/components/icons/IconBili.vue";
import type { AvatarConfig } from "@/types/avatar";

const customForm = reactive<AvatarConfig>({
  showBadge: true,
  badgeStyle: "normal",
  offsetX: 0,
  offsetY: 0,
  filter: 50,
  badgeSize: 0,
});

function resetPosition() {
  customForm.offsetX = 0;
  customForm.offsetY = 0;
}

const cropperRef = ref<{ getCropData: () => Promise<string> | undefined } | null>(null);
const uploadRef = ref<{ handleReset: () => void } | null>(null);
const showModal = ref(false);
const showResModal = ref(false);
const imageBase64 = ref("");
const cropImageBase64 = ref("");

function handleUpload(base: string) {
  imageBase64.value = base;
  showModal.value = true;
}

function handleReset() {
  imageBase64.value = "";
  cropImageBase64.value = "";
  showModal.value = false;
  uploadRef.value?.handleReset();
}

async function handleCrop() {
  const result = await cropperRef.value?.getCropData();

  if (!result) {
    return;
  }

  showModal.value = false;
  cropImageBase64.value = result;
}

const urls = ref("");
function handleImage() {
  const el = document.getElementById("PreviewShell");
  html2canvas(el as HTMLElement).then((canvas) => {
    urls.value = canvas.toDataURL("image/png");
    showResModal.value = true;
  });
}
function handleDownload() {
  // const el = document.getElementById("PreviewShell");
  // html2canvas(el as HTMLElement).then((canvas) => {
  const url = urls.value;
  const a = document.createElement("a");
  a.href = url;
  a.download = "头像.png";
  a.click();
  // });
}

function toLink() {
  window.open("https://github.com/KurumiWWW/stronghold-protocol-avatar");
}
function toBili() {
  window.open("https://www.bilibili.com/video/BV1b5XWB1Eyk");
}

function onAfterLeave() {
  imageBase64.value = "";
  uploadRef.value?.handleReset();
}
function onAfterLeaveRes() {
  urls.value = "";
}
</script>
