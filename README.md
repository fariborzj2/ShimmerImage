# ShimmerImage.vue

## Short Description
`ShimmerImage` is a Nuxt image component with shimmer loader, optional lazy loading, priority for key images, responsive formats (WebP/AVIF), and customizable placeholders. Optimized for fast loading and improved LCP.

---

## Overview
`ShimmerImage` is a versatile image component for **Nuxt 3/4** that provides:

- **Shimmer loader** during image loading  
- **Optional lazy loading**  
- **Priority loading** for important images (eager + fetchpriority high)  
- Support for **responsive formats** like WebP and AVIF  
- Full attribute customization via `img-attrs`  
- Placeholder support (image or blur)  

This component is ideal for **blogs, galleries, and content-heavy pages**, improving **LCP and perceived performance**.

---

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `src` | `String` | — | Image source URL (required) |
| `alt` | `String` | `''` | Alternative text |
| `width` | `Number \| String` | `1200` | Image width |
| `height` | `Number \| String` | `675` | Image height |
| `index` | `Number` | `0` | Image position in a list (used for priority calculation) |
| `priorityCount` | `Number` | `4` | Number of first images with high priority |
| `lazy` | `Boolean` | `false` | Enable lazy loading. Default is false (eager) |
| `format` | `String \| Array` | `['avif','webp']` | Image formats for NuxtPicture |
| `widths` | `Array` | `[320,480,640,800,1200]` | Responsive image widths |
| `sizes` | `String` | `'(max-width: 768px) 100vw, 1200px'` | Sizes attribute for responsive images |
| `imgAttrs` | `Object` | `{}` | Override image attributes like `loading` or `fetchpriority` |
| `placeholder` | `String \| Number` | `'/images/default-placeholder.png'` | Placeholder image URL or blur value |

---

## Default Behavior

- Images are **eager-loaded** unless `lazy=true`  
- Images with `index < priorityCount` are **priority loaded** (fetchpriority=high)  
- Shimmer loader is displayed until the image fully loads  
- `imgAttrs` can override all default attributes

---

## Usage Examples

### 1. Main blog image with high priority
```vue
<ShimmerImage 
  :src="imageUrl" 
  :alt="post.title" 
  :width="1200"
  :height="675"
  :index="0"
  :priority-count="4"
/>
```

### 2. Author avatar with lazy loading
```vue
<ShimmerImage
  :src="autorPhoto"
  alt="Author"
  width="40"
  height="40"
  :lazy="true"
/>
```

### 3. Override img-attrs manually
```vue
<ShimmerImage
  :src="imageUrl"
  alt="Example Image"
  width="1200"
  height="675"
  format="avif,webp"
  :img-attrs="{ loading: 'eager', fetchpriority: 'auto' }"
/>
```

### 4. Looping through multiple images
```vue
  <div v-for="(image, i) in images" :key="i">
    <ShimmerImage
      :src="image.url"
      :alt="image.title"
      :index="i"
      :priority-count="3"
      :lazy="i >= 3"
    />
  </div>
```

### 5. Custom placeholder
```vue
<ShimmerImage
  :src="imageUrl"
  alt="Example"
  width="1200"
  height="675"
  :placeholder="'/images/placeholder-small.jpg'"
/>
```

## Recommendations

- Use **low index** and **high `priorityCount`** for Hero or above-the-fold images to optimize **LCP**.  
- **Lazy-load** smaller, non-critical images to improve **page performance**.  
- Adjust `widths` and `sizes` for **responsive images** and better **bandwidth usage**.  
- Override `imgAttrs` for custom `loading` or `fetchpriority` behavior.

---

## CSS / Shimmer

- `.shimmer-wrapper` is the **main container**.  
- `.shimer-img` class is **active while the image is loading** and shows **shimmer animation**.  

### Keyframes animation for shimmer:

```css
@keyframes shimer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
@media (prefers-reduced-motion: reduce) {
  .shimmer-wrapper.shimer-img {
    animation: none;
  }
}
```

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.
