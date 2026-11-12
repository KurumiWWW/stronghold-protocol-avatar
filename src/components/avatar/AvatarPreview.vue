<script setup lang="ts">
import type { AvatarConfig } from "@/types/avatar";

defineProps<{
  img?: string;
  config: AvatarConfig;
}>();
</script>

<template>
  <div class="mt-2.5 w-full max-w-lg" id="PreviewShell">
    <div class="pointer-events-none relative aspect-square w-full overflow-hidden bg-[rgba(50,50,50,1)]">
      <img class="absolute bottom-[10%] left-[10%] z-10 block w-[40%]" src="/111.png" alt="" />
      <img class="absolute left-[8%] top-[12.5%] z-10 block w-[40%]" src="/112.png" alt="" />
      <div
        class="absolute z-10 aspect-square w-[40%] origin-center bg-[url('/av-icon.png')] bg-size-[100%_100%] bg-no-repeat"
        :style="{
          top: `${-6 + config.offsetY}%`,
          right: `${-6 + config.offsetX}%`,
          transform: `scale(${1 + config.badgeSize * 0.01})`,
        }"
        v-if="config.showBadge"
      ></div>
      <div class="absolute left-1/2 top-1/2 z-0 aspect-square h-[95%] w-[95%] -translate-x-1/2 -translate-y-1/2 rounded-full">
        <div
          class="absolute inset-0 z-10 -ml-[1%] -mt-[1%] h-[102%] w-[102%] bg-[radial-gradient(circle,rgba(255,255,255,0)_0%,rgba(255,255,255,0)_33%,rgba(50,50,50,0.1)_40%,rgba(50,50,50,0.4)_50%,rgba(50,50,50,1)_65%)]"
        ></div>
        <div
          class="absolute inset-0 z-9 overflow-hidden"
          :style="{
            background: `rgba(48,219,177,${config.filter / 200})`,
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
