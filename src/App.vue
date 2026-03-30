<template>
  <n-config-provider :theme-overrides="themeOverrides">
    <n-message-provider>
      <n-modal-provider>
        <div class="app-shell">
          <div class="app-content">
            <p class="app-title">卫戍协议头像生成器</p>
            <p class="app-subtitle">卫吗？</p>
            <image-upload @upload="handleUpload" ref="uploadRef" />
            <avatar-preview :img="cropImageBase64" />
            <div class="action-row">
              <n-button @click="handleReset">重置</n-button>
              <n-button type="primary" @click="handleImage" :disabled="!cropImageBase64"
                >生成
              </n-button>
            </div>
            <div class="link">
              <n-button quaternary round @click="toLink">
                <template #icon>
                  <n-icon size="20">
                    <LogoGithub />
                  </n-icon>
                </template>
                <span>Github</span>
              </n-button>
            </div>
          </div>
        </div>
        <n-modal v-model:show="showModal" :on-after-leave="onAfterLeave">
          <div class="cropper-modal">
            <vue-cropper ref="cropperRef" class="cropper-panel" :img="imageBase64" />
            <n-flex justify="end" class="cropper-actions">
              <n-button type="primary" @click="handleCrop">提交</n-button>
            </n-flex>
          </div>
        </n-modal>
        <n-modal v-model:show="showResModal" :on-after-leave="onAfterLeaveRes">
          <div class="result-modal">
            <img :src="urls" alt="" style="display: block" />
            <n-flex justify="start" class="cropper-actions">
              <p>tips：移动端若出现导出按钮无法使用的情况，可以通过手动长按保存至本地</p>
              <n-button style="width: 100%" type="primary" @click="handleDownload">导出</n-button>
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

function onAfterLeave() {
  imageBase64.value = "";
  uploadRef.value?.handleReset();
}
function onAfterLeaveRes() {
  urls.value = "";
}
</script>

<style scoped>
.app-shell {
  min-height: 100vh;
  padding: clamp(24px, 4vw, 40px) 16px;
  display: flex;
  justify-content: center;
}

.app-content {
  width: min(100%, 640px);
  display: flex;
  flex-direction: column;
  align-items: center;
  /*gap: clamp(16px, 3vw, 24px);*/
}

.app-title {
  margin: 0;
  text-align: center;
  color: rgb(14 116 144);
  font-size: clamp(2rem, 4vw, 2.25rem);
  font-weight: 700;
}

.app-subtitle {
  margin: 0;
  text-align: center;
  color: rgb(156 163 175);
  font-size: clamp(1rem, 2.5vw, 1.25rem);
}

.action-row {
  width: min(100%, 512px);
  display: flex;
  justify-content: flex-end;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 20px;
}

.cropper-modal {
  width: min(90vw, 480px);
  background: #fff;
  border-radius: 20px;
  padding: clamp(16px, 4vw, 24px);
  box-sizing: border-box;
}

.result-modal {
  width: min(90vw, 530px);
  background: #fff;
  border-radius: 20px;
  padding: clamp(16px, 4vw, 24px);
  box-sizing: border-box;
}

.cropper-panel {
  width: 100%;
  aspect-ratio: 1;
  display: block;
  margin: 0 auto;
}

.cropper-actions {
  margin-top: 12px;
}

.link {
  margin-top: 20px;
}

.link span {
  font-size: 16px;
}

@media (max-width: 640px) {
  .action-row {
    justify-content: stretch;
  }

  .action-row :deep(.n-button) {
    flex: 1 1 160px;
  }
}
</style>
