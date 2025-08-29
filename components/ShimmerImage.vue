<script setup>
const props = defineProps({
  src: { type: String, required: true },
  alt: { type: String, default: '' },
  width: { type: [Number, String], default: 1200 },
  height: { type: [Number, String], default: 675 },
  index: { type: Number, default: 0 },
  priorityCount: { type: Number, default: 4 }, // تعداد تصاویر با priority بالا
  lazy: { type: Boolean, default: false },   // ← lazy پیش‌فرض غیرفعال
  format: { type: [String, Array], default: () => ['avif', 'webp'] },
  widths: { type: Array, default: () => [320, 480, 640, 800, 1200] },
  sizes: { type: String, default: '(max-width: 768px) 100vw, 1200px' },
  imgAttrs: { type: Object, default: () => ({}) },
  placeholder: { type: [String, Number], default: 20 },
})

// تعیین priority براساس index و تعداد مشخص‌شده
const isPriority = computed(() => props.index < props.priorityCount);

// کنترل loading
const finalImgAttrs = computed(() => ({
  loading: props.lazy ? 'lazy' : 'eager',       // ← اگر lazy=true باشد lazy می‌شود
  decoding: 'async',
  fetchpriority: isPriority.value ? 'high' : 'auto',
  ...props.imgAttrs,
}));

const finalFormat = computed(() =>
  Array.isArray(props.format) ? props.format.join(',') : props.format
);

const isLoaded = ref(false);

</script>

<template>
  <div class="shimmer-wrapper" :class="{ 'shimer-img': !isLoaded }">
    <NuxtPicture
      :src="src"
      :alt="alt"
      :width="width"
      :height="height"
      :format="finalFormat"
      :widths="widths"
      :sizes="sizes"
      :img-attrs="finalImgAttrs"
      :placeholder="placeholder"
      class="d-block image"
      @load="isLoaded = true"
    />
  </div>
</template>

<style scoped>
.shimmer-wrapper {
  position: relative;
  display: block;
  overflow: hidden;
}

/* Shimmer animation */
.shimer-img {
  position: relative;
  overflow: hidden;
}
.shimer-img::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, var(--color-neutral) 25%, var(--color-neutral-darker) 50%, var(--color-neutral) 75%);
  animation: shimer 1.5s infinite;
  background-size: 200% 100%;
  z-index: 0;
}
.shimmer-wrapper:not(.shimer-img) .image {
  opacity: 1;
}

/* Keyframes shimmer */
@keyframes shimer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  .shimmer-wrapper.shimer-img {
    animation: none;
  }
}
</style>
