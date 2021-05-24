<template>
  <div class="columns do-center">
    <div class="column is-8">
      <img v-if="image" :src="getImageUrl(image)" alt="">
    </div>
    <div class="column is-4">
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
</template>
<script>
import Label from "@/components/Label";
export default {
  name: 'Highlight',
  components: {Label},
  props: ['title', 'description', 'action', 'image', 'labels'],
  methods: {
    getImageUrl(name) {
      return require(`@/assets/${name}`)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
