<template>
  <div class="is-flex is-align-items-center contact-link">
    <a :href="link" target="_blank" class="is-flex is-align-items-center is-clickable">
      <figure class="image is-32x32">
        <img loading="lazy" alt="" :src="getIconUrl(icon)">
      </figure>
      <p v-if="link" class="has-text-weight-semibold">{{ name }}</p>
      <p v-else class="has-text-weight-semibold">{{ text }}</p>
    </a>
  </div>
</template>
<script>
export default {
  name: 'ContactLink',
  props: ['name', 'icon', 'link', 'text'],
  methods: {
    getIconUrl(name) {
      return require(`@/assets/icons/${name}.png`)
    }
  }
}
</script>

<style lang="scss" scoped>
@media screen and (max-width: 768px) {
  .contact-link {
    justify-content: center;
  }
}
a {
  color: #fff;
  &:hover {
    p {
      color: #fff;
    }
  }
}
</style>
