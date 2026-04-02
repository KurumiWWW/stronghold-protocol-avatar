<script setup lang="ts">
import {
  NFlex,
  NSwitch,
  NCollapse,
  NCollapseItem,
  NForm,
  NFormItem,
  NSlider,
  NButton,
  NIcon,
} from "naive-ui";
import { Refresh } from "@vicons/ionicons5";
import { reactive } from "vue";
defineProps({
  img: String,
});

const customForm = reactive({
  showBadge: true,
  offsetX: 0,
  offsetY: 0,
  filter: 50,
  badgeSize: 0,
});

function resetPosition() {
  customForm.offsetX = 0;
  customForm.offsetY = 0;
}
</script>

<template>
  <div class="preview-form">
    <n-collapse :default-expanded-names="['1']">
      <n-collapse-item title="配置菜单" name="1">
        <n-form ref="formRef" :model="customForm" label-placement="left" label-width="auto">
          <n-form-item label="展示角标" path="showBadge">
            <n-switch v-model:value="customForm.showBadge" />
          </n-form-item>
          <n-form-item label="角标大小" path="badgeSize">
            <n-slider v-model:value="customForm.badgeSize" :step="1" :min="-100" :max="100" />
          </n-form-item>
          <n-flex align="center">
            角标位置（适配QQ等圆形头像）
            <n-button circle size="small" @click="resetPosition">
              <template #icon>
                <n-icon><Refresh /></n-icon>
              </template>
            </n-button>
          </n-flex>
          <n-form-item label="X">
            <n-slider v-model:value="customForm.offsetX" :step="1" :min="-100" :max="100" />
          </n-form-item>
          <n-form-item label="Y">
            <n-slider v-model:value="customForm.offsetY" :step="1" :min="-100" :max="100" />
          </n-form-item>
          <n-form-item label="滤镜强度" path="filter">
            <n-slider v-model:value="customForm.filter" :step="1" :min="0" :max="100" />
          </n-form-item>
        </n-form>
      </n-collapse-item>
    </n-collapse>
  </div>
  <div class="preview-shell" id="PreviewShell">
    <div class="preview-stage">
      <!--      <div class="av-text preview-layer"></div>-->
      <!--      <div class="av-text-1 preview-layer"></div>-->
      <img class="av-text-1" src="/111.png" alt="" />
      <img class="av-text-2" src="/112.png" alt="" />
      <div
        class="av-icon"
        :style="{
          top: `${-6 + customForm.offsetY}%`,
          right: `${-6 + customForm.offsetX}%`,
          transform: `scale(${1 + customForm.badgeSize * 0.01})`,
        }"
        v-if="customForm.showBadge"
      ></div>
      <div class="preview-avatar">
        <div class="av-cover preview-layer"></div>
        <div
          class="preview-grid"
          :style="{
            background: `rgba(48,219,177,${customForm.filter / 200})`,
          }"
        >
          <div class="preview-grid-line" v-for="item in 150" :key="item"></div>
        </div>
        <img v-if="img" :src="img" alt="avatar preview" class="preview-image" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.preview-form {
  /*color: #30dbb1;*/
  width: min(100%, 512px);
  margin-top: 6px;
}

.preview-shell {
  width: min(100%, 512px);
  margin-top: 10px;
}

.preview-stage {
  pointer-events: none;
  position: relative;
  width: 100%;
  aspect-ratio: 1;
  overflow: hidden;
  background: rgba(50, 50, 50, 1);
  /*background: rgba(0, 0, 0, 1);*/
}

.preview-layer {
  position: absolute;
  inset: 0;
  z-index: 10;
}

.av-text {
  /*background: url("/av-text-1.png") no-repeat 30px center;
  background-size: 70% 70%;*/
  background: url("/av-text.png") no-repeat;
  background-size: 100% 100%;
}
.av-text-1 {
  display: block;
  width: 40%;
  position: absolute;
  z-index: 10;
  bottom: 10%;
  left: 10%;
}
.av-text-2 {
  display: block;
  width: 40%;
  position: absolute;
  z-index: 10;
  top: 12.5%;
  left: 8%;
}

.av-icon {
  position: absolute;
  z-index: 10;
  aspect-ratio: 1;
  background: url("/av-icon.png") no-repeat;
  background-size: 100% 100%;
  width: 40%;
  transform-origin: center;
}

.preview-avatar {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 0;
  width: 95%;
  height: 95%;
  aspect-ratio: 1;
  border-radius: 9999px;
  transform: translate(-50%, -50%);
  /*overflow: hidden;*/
}

.av-cover {
  background: radial-gradient(
    circle,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0) 33%,
    rgba(50, 50, 50, 0.1) 40%,
    rgba(50, 50, 50, 0.4) 50%,
    rgba(50, 50, 50, 1) 65%
  );
  width: 102%;
  height: 102%;
  margin-top: -1%;
  margin-left: -1%;
}

.preview-grid {
  position: absolute;
  inset: 0;
  z-index: 9;
  overflow: hidden;
}

.preview-grid-line {
  width: 100%;
  height: 3px;
  margin-top: 3px;
  /*  background: rgba(40, 40, 40, 0.1);
  background: rgba(255, 255, 255, 0.1);*/
  background: rgba(48, 219, 177, 0.15);
}

.preview-image {
  position: absolute;
  inset: 0;
  z-index: 0;
  width: 100%;
  height: 100%;
}
</style>
