<template>
  <n-config-provider :theme-overrides="themeOverrides">
    <n-message-provider>
      <n-modal-provider>
        <div class="flex min-h-screen justify-center px-4 py-[clamp(24px,4vw,24px)]">
          <div class="flex w-full max-w-[640px] flex-col items-center">
            <p class="m-0 text-center text-[rgb(14_116_144)] [font-size:clamp(2rem,4vw,2.25rem)] font-bold">
              卫戍协议头像生成器
            </p>
            <p class="m-0 text-center text-[rgb(156_163_175)] [font-size:clamp(1rem,2.5vw,1.25rem)]">
              卫吗？
            </p>
            <image-upload @upload="handleUpload" ref="uploadRef" />
            <avatar-preview :img="cropImageBase64" />
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
        <n-modal v-model:show="showModal" :on-after-leave="onAfterLeave">
          <div class="w-[min(90vw,480px)] rounded-[20px] bg-white p-[clamp(16px,4vw,24px)]">
            <vue-cropper
              ref="cropperRef"
              class="mx-auto block aspect-square w-full"
              :img="imageBase64"
            />
            <n-flex justify="end" class="mt-3">
              <n-button type="primary" @click="handleCrop">提交</n-button>
            </n-flex>
          </div>
        </n-modal>
        <n-modal v-model:show="showResModal" :on-after-leave="onAfterLeaveRes">
          <div class="w-[min(90vw,530px)] rounded-[20px] bg-white p-[clamp(16px,4vw,24px)]">
            <img :src="urls" alt="" class="block" />
            <n-flex justify="start" class="mt-3">
              <p>tips：移动端若出现导出按钮无法使用的情况，可以通过手动长按保存至本地</p>
              <n-button class="w-full" type="primary" @click="handleDownload">导出</n-button>
            </n-flex>
          </div>
        </n-modal>
      </n-modal-provider>
    </n-message-provider>
  </n-config-provider>
</template>

<script setup lang="ts">
import html2canvas from "html2canvas";
import {
  NButton,
  NConfigProvider,
  NFlex,
  NMessageProvider,
  NModal,
  NModalProvider,
  type GlobalThemeOverrides,
  NIcon,
} from "naive-ui";
import { VueCropper } from "cropper-next-vue";
import { ref } from "vue";
import AvatarPreview from "@/components/avatarPreview.vue";
import ImageUpload from "@/components/imageUpload.vue";
import { LogoGithub } from "@vicons/ionicons5";
import IconBili from "@/components/iconBili.vue";

const themeOverrides: GlobalThemeOverrides = {
  common: {
    primaryColor: "#007595FF",
  },
};

const cropperRef = ref<{ getCropData: () => Promise<string> } | null>(null);
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
