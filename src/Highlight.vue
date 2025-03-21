<template>
  <div class="highlight-container">
    <div class="columns is-multiline do-center-y showcase-item" ref="showcaseItem">
      <div class="column is-8-desktop">
        <div class="images-container">
          <img loading="lazy" :src="getImageUrl(images[0])" class="primary-image" :class="{'fade-in-top': isActive, 'fade-out-bottom': isLeaving}" alt="">
        </div>
      </div>
      <div class="column is-4-desktop">
        <div v-if="labels.length" :class="{'fade-in-bottom': isActive, 'fade-out-top': isLeaving}">
          <Label v-for="(label, index) in labels" :key="index" :text="label"/>
        </div>
        <h1 class="is-size-4 mb-4" :class="{'fade-in-bottom': isActive, 'fade-out-top': isLeaving}">{{ title }}</h1>
        <p class="mb-4" :class="{'fade-in-bottom': isActive, 'fade-out-top': isLeaving}">
          {{ description }}
        </p>
        <a v-if="action" :href="action.link" target="_blank" class="button is-outlined" :class="{'fade-in-bottom': isActive, 'fade-out-top': isLeaving}">{{ action.text }}</a>
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
      isActive: true, // Initialize as true to show content by default
      isLeaving: false,
      observer: null
      // Removed hasBeenShown flag
    }
  },
  mounted() {
    this.$nextTick(() => {
      // Delay setup to ensure DOM is fully rendered
      setTimeout(() => {
        this.setupIntersectionObserver();
      }, 100);
    });
  },
  beforeUnmount() {
    if (this.observer) {
      this.observer.disconnect();
    }
  },
  methods: {
    getImageUrl(name) {
      return require(`@/assets/${name}`)
    },
    setupIntersectionObserver() {
      const options = {
        root: null,
        rootMargin: '0px',
        threshold: [0.1, 0.5] // Lowered thresholds for better detection
      };
      
      this.observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          // Element is entering viewport
          if (entry.isIntersecting) {
            this.isActive = true;
            this.isLeaving = false;
            // Removed the code that sets hasBeenShown
          } 
          // Element is leaving viewport - reverted to original behavior
          else {
            this.isLeaving = true;
            this.isActive = false;
          }
        });
      }, options);
      
      if (this.$refs.showcaseItem) {
        this.observer.observe(this.$refs.showcaseItem);
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.highlight-container {
  min-height: 80vh; /* Reduced from 100vh to 80vh */
  position: relative;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px; /* Added small margin between highlights */
  max-width: 100%;
  overflow-x: hidden; /* Prevent horizontal overflow */
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
}

.primary-image {
  position: relative;
  width: 100%;
  max-width: 100%; /* Ensure image doesn't exceed container width */
  transition: opacity 0.8s ease, transform 0.8s ease;
}

// Fade animations
.fade-in-top {
  opacity: 1;
  transform: translateY(0);
}

.fade-out-bottom {
  opacity: 0;
  transform: translateY(30px);
}

.fade-in-bottom {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.fade-out-top {
  opacity: 0;
  transform: translateY(-30px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

// Initial state for animations - be more specific to avoid affecting all elements
.showcase-item .primary-image:not(.fade-in-top):not(.fade-out-bottom) {
  opacity: 0;
  transform: translateY(-30px);
}

.showcase-item .column.is-4-desktop h1:not(.fade-in-bottom):not(.fade-out-top),
.showcase-item .column.is-4-desktop p:not(.fade-in-bottom):not(.fade-out-top),
.showcase-item .column.is-4-desktop a:not(.fade-in-bottom):not(.fade-out-top),
.showcase-item .column.is-4-desktop div:not(.column):not(.fade-in-bottom):not(.fade-out-top) {
  opacity: 0;
  transform: translateY(30px);
}

@media screen and (max-width: 768px) {
  .do-center-y {
    display: block;
  }
  
  .showcase-item {
    position: relative;
    height: auto;
    margin-bottom: 3vh; /* Reduced from 5vh to 3vh */
    width: 100%;
    max-width: 100%;
    padding: 0 15px; /* Add padding instead of margins for content */
  }
  
  .highlight-container {
    height: auto;
    margin-bottom: 3vh; /* Reduced from 5vh to 3vh */
    width: 100%;
    padding: 0;
  }
  
  /* Add this to control body/html overflow on mobile */
  :global(body), :global(html) {
    overflow-x: hidden;
    width: 100%;
    position: relative;
  }
}
</style>
