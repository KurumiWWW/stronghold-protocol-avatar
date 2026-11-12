<template>
  <n-config-provider :theme-overrides="themeOverrides">
    <n-message-provider>
      <n-modal-provider>
        <avatar-generator-view />
      </n-modal-provider>
    </n-message-provider>
  </n-config-provider>
</template>

<script setup lang="ts">
import {
  NConfigProvider,
  NMessageProvider,
  NModalProvider,
  type GlobalThemeOverrides,
} from "naive-ui";
import AvatarGeneratorView from "@/views/AvatarGeneratorView.vue";

const themeOverrides: GlobalThemeOverrides = {
  common: {
    primaryColor: "#007595FF",
  },
};
</script>
