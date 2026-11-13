<script setup lang="ts">
import type { AvatarConfig } from "@/types/avatar";

const baseUrl = import.meta.env.BASE_URL;
const badgeImages: Record<AvatarConfig["badgeStyle"], string> = {
  normal: `${baseUrl}av-icon.png`,
  advanced: `${baseUrl}av-icon-m.png`,
};

defineProps<{
  img?: string;
  config: AvatarConfig;
}>();
</script>

<template>
  <div class="mt-2.5 w-full max-w-lg" id="PreviewShell">
    <div class="pointer-events-none relative aspect-square w-full overflow-hidden bg-[rgba(50,50,50,1)]">
      <div class="avatar-portrait absolute inset-[2.5%] z-0">
        <img v-if="img" :src="img" alt="avatar preview" class="absolute inset-0 h-full w-full" />
        <div
          class="absolute inset-0"
          :style="{
            background: `rgba(48,219,177,${config.filter / 200})`,
          }"
        ></div>
        <!-- 扫描线与头像共用遮罩，只在头像区域显示并随边缘渐隐。 -->
        <div class="avatar-scanlines absolute inset-0 z-10"></div>
      </div>
      <img
        class="absolute bottom-[10%] left-[10%] z-20 block w-[40%]"
        :src="`${baseUrl}111.png`"
        alt=""
      />
      <img
        class="absolute left-[8%] top-[12.5%] z-20 block w-[40%]"
        :src="`${baseUrl}112.png`"
        alt=""
      />
      <!-- 角标使用独立的最高层级，始终位于头像、滤镜和装饰之上。 -->
      <img
        v-if="config.showBadge"
        :src="badgeImages[config.badgeStyle]"
        alt=""
        class="absolute z-30 h-[60%] w-[60%] origin-center"
        :style="{
          top: `${-14 + config.offsetY}%`,
          right: `${-14 + config.offsetX}%`,
          transform: `scale(${1 + config.badgeSize * 0.01})`,
        }"
      />
    </div>
  </div>
</template>

<style scoped>
.avatar-portrait {
  /* 百分比相对于圆的半径：中心保持原色，外围逐渐透明并露出底色。 */
  --fade-start: 70%;
  --fade-end: 100%;
  --portrait-mask: radial-gradient(
    circle closest-side at center,
    #000 var(--fade-start),
    transparent var(--fade-end)
  );

  -webkit-mask-image: var(--portrait-mask);
  mask-image: var(--portrait-mask);
  mask-mode: alpha;
}

.avatar-scanlines {
  /* 使用头像区域高度的比例定义周期，缩放预览时保持相同的扫描线数量。 */
  --scan-period: 1.8%;
  --scan-width: 25%;

  /* 平铺普通渐变，避免导出时丢失重复渐变；扫描线强度不随滤镜滑块变化。 */
  background-image: linear-gradient(
    to bottom,
    rgba(48, 219, 177, 0.3) 0%,
    rgba(48, 219, 177, 0.3) var(--scan-width),
    transparent var(--scan-width),
    transparent 100%
  );
  background-size: 100% var(--scan-period);
  background-repeat: repeat-y;
}
</style>
