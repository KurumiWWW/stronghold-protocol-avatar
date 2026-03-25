<script setup lang="ts">
defineProps({
  img: String,
});
</script>

<template>
  <div class="preview-shell" id="PreviewShell">
    <div class="preview-stage">
      <div class="av-text preview-layer"></div>
      <div class="av-icon"></div>
      <div class="preview-avatar">
        <div class="av-cover preview-layer"></div>
        <div class="preview-grid">
          <div class="preview-grid-line" v-for="item in 100" :key="item"></div>
        </div>
        <img v-if="img" :src="img" alt="avatar preview" class="preview-image" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.preview-shell {
  width: min(100%, 512px);
}

.preview-stage {
  pointer-events: none;
  position: relative;
  width: 100%;
  aspect-ratio: 1;
  //overflow: hidden;
  background: rgba(68, 68, 68, 1);
}

.preview-layer {
  position: absolute;
  inset: 0;
  z-index: 10;
}

.av-text {
  background: url("/av-text.png") no-repeat;
  background-size: 100% 100%;
}

.av-icon {
  position: absolute;
  top: -6%;
  right: -6%;
  z-index: 10;
  width: 40%;
  aspect-ratio: 1;
  background: url("/av-icon.png") no-repeat;
  background-size: 100% 100%;
}

.preview-avatar {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 0;
  width: 78%;
  height: 78%;
  aspect-ratio: 1;
  border-radius: 9999px;
  transform: translate(-50%, -50%);
  //overflow: hidden;
}

.av-cover {
  background: radial-gradient(circle, rgba(68, 68, 68, 0) 0%, rgba(68, 68, 68, 1) 70%);
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
  background: rgba(19, 219, 157, 0.3);
}

.preview-grid-line {
  width: 100%;
  height: 2px;
  margin-top: 2px;
  background: rgba(68, 68, 68, 0.5);
}

.preview-image {
  position: absolute;
  inset: 0;
  z-index: 0;
  width: 100%;
  height: 100%;
}
</style>
