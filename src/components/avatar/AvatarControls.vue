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
import type { AvatarConfig } from "@/types/avatar";

const showBadge = defineModel<AvatarConfig["showBadge"]>("showBadge", { required: true });
const badgeSize = defineModel<AvatarConfig["badgeSize"]>("badgeSize", { required: true });
const offsetX = defineModel<AvatarConfig["offsetX"]>("offsetX", { required: true });
const offsetY = defineModel<AvatarConfig["offsetY"]>("offsetY", { required: true });
const filter = defineModel<AvatarConfig["filter"]>("filter", { required: true });
const emit = defineEmits<{ resetPosition: [] }>();
</script>

<template>
  <div class="mt-1.5 w-full max-w-lg">
    <n-collapse :default-expanded-names="['1']">
      <n-collapse-item title="配置菜单" name="1">
        <n-form
          ref="formRef"
          :model="{ showBadge, badgeSize, offsetX, offsetY, filter }"
          label-placement="left"
          label-width="auto"
        >
          <n-form-item label="展示角标" path="showBadge">
            <n-switch v-model:value="showBadge" />
          </n-form-item>
          <n-form-item label="角标大小" path="badgeSize">
            <n-slider v-model:value="badgeSize" :step="1" :min="-100" :max="100" />
          </n-form-item>
          <n-flex align="center">
            角标位置（适配QQ等圆形头像）
            <n-button circle size="small" @click="emit('resetPosition')">
              <template #icon>
                <n-icon><Refresh /></n-icon>
              </template>
            </n-button>
          </n-flex>
          <n-form-item label="X">
            <n-slider v-model:value="offsetX" :step="1" :min="-100" :max="100" />
          </n-form-item>
          <n-form-item label="Y">
            <n-slider v-model:value="offsetY" :step="1" :min="-100" :max="100" />
          </n-form-item>
          <n-form-item label="滤镜强度" path="filter">
            <n-slider v-model:value="filter" :step="1" :min="0" :max="100" />
          </n-form-item>
        </n-form>
      </n-collapse-item>
    </n-collapse>
  </div>
</template>
