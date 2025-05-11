<template>
  <div class="highlight-container">
    <div class="columns is-multiline do-center-y showcase-item" ref="showcaseItem">
      <div class="column is-8-desktop">
        <div class="images-container">
          <img loading="lazy" :src="getImageUrl(images[0])" class="primary-image" alt="">
        </div>
      </div>
      <div class="column is-4-desktop">
        <div v-if="labels.length">
          <Label v-for="(label, index) in labels" :key="index" :text="label"/>
        </div>
        <h1 class="is-size-4 mb-4">{{ title }}</h1>
        <p class="mb-4">
          {{ description }}
        </p>
        <a v-if="action" :href="action.link" target="_blank" class="button is-outlined">{{ action.text }}</a>
      </div>
    </div>
  </div>
</template>
<script>
import Label from "@/components/Label";
export default {
  name: 'Highlight',
  components: {Label},
  props: ['title', 'description', 'action', 'images', 'labels'],
  data() {
    return {
      // No animation states needed
    }
  },
  methods: {
    getImageUrl(name) {
      return require(`@/assets/${name}`)
    }
  }
}
</script>

<style lang="scss" scoped>
.highlight-container {
  position: relative;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.showcase-item {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 1;
  width: 100%;
}

.images-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 100%; /* Ensure images don't exceed container width */
  img {
    width: 100%;
    height: auto;
    object-fit: contain;
  }
}

.primary-image {
  position: relative;
  width: 100%;
  max-width: 100%; /* Ensure image doesn't exceed container width */
}

.showcase-item {
  opacity: 1;
  transform: translateY(0);
}

@media screen and (max-width: 768px) {
  .do-center-y {
    display: block;
  }
  
  .showcase-item {
    position: relative;
    margin-bottom: 5vh;
    width: 100%;
  }
  
  .highlight-container {
    width: 100%;
    padding: 0;
  }
}
</style>
