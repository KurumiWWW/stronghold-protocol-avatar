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
  <div class="mt-1.5 w-full max-w-lg">
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
  <div class="mt-2.5 w-full max-w-lg" id="PreviewShell">
    <div class="pointer-events-none relative aspect-square w-full overflow-hidden bg-[rgba(50,50,50,1)]">
      <img class="absolute bottom-[10%] left-[10%] z-10 block w-[40%]" src="/111.png" alt="" />
      <img class="absolute left-[8%] top-[12.5%] z-10 block w-[40%]" src="/112.png" alt="" />
      <div
        class="absolute z-10 aspect-square w-[40%] origin-center bg-[url('/av-icon.png')] bg-size-[100%_100%] bg-no-repeat"
        :style="{
          top: `${-6 + customForm.offsetY}%`,
          right: `${-6 + customForm.offsetX}%`,
          transform: `scale(${1 + customForm.badgeSize * 0.01})`,
        }"
        v-if="customForm.showBadge"
      ></div>
      <div class="absolute left-1/2 top-1/2 z-0 aspect-square h-[95%] w-[95%] -translate-x-1/2 -translate-y-1/2 rounded-full">
        <div
          class="absolute inset-0 z-10 -ml-[1%] -mt-[1%] h-[102%] w-[102%] bg-[radial-gradient(circle,rgba(255,255,255,0)_0%,rgba(255,255,255,0)_33%,rgba(50,50,50,0.1)_40%,rgba(50,50,50,0.4)_50%,rgba(50,50,50,1)_65%)]"
        ></div>
        <div
          class="absolute inset-0 z-9 overflow-hidden"
          :style="{
            background: `rgba(48,219,177,${customForm.filter / 200})`,
          }"
        >
          <div
            class="mt-0.75 h-0.75 w-full bg-[rgba(48,219,177,0.15)]"
            v-for="item in 150"
            :key="item"
          ></div>
        </div>
        <img v-if="img" :src="img" alt="avatar preview" class="absolute inset-0 z-0 h-full w-full" />
      </div>
    </div>
  </div>
</template>
