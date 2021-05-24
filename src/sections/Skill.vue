<template>
  <div class="columns is-multiline skills">
    <div class="column is-5-desktop">
      <div class="columns is-multiline icons-wrapper">
        <div v-for="(skillName, index) in skillImages" :key="index" class="column is-narrow icon-skill">
          <figure class="image is-64x64">
            <img :src="getIconUrl(skillName)" alt="">
          </figure>
        </div>
      </div>
    </div>
    <div class="column is-7-desktop">
      <h3 class="title is-3">Skills</h3>
      <p class="mb-3">
        Normally I work with <b>HTML</b>, <b>CSS</b> (and <b>SCSS</b>), <b>JavaScript</b> almost all the time. I
        love being a full-stack web developer (but I love being a backend guy a little bit more 😁)
      </p>
      <p class="mb-3">
        I've been working with many CSS frontend framework such as <b>Bootstrap</b>, <b>Bulma</b>, and <b>Foundation</b>.
        So I'm familiar with the concept of <b>grid system</b>.
        I also love to use JavaScript frontend framework such as <b>Vue.js</b> and <b>React.js</b>. I've created many
        Vue.js web applications while I was a college student for many types of purposes.
      </p>
      <p class="mb-3">
        In the backend world, I use <b>JavaScript</b> (<b>Node.js</b>) with <b>Express.js</b> mainly. I'm
        comfortable with <b>PHP</b> and its popular framework such as <b>Wordpress</b> and <b>Laravel</b>. And
        for storing data to a <b>database</b>. I can use, write, and optimize <b>SQL</b> query to get a better overall performance.
      </p>
      <p>
        Besides all that, I have experienced on <b>linux server administration</b>. I understand a web service <b>deployment</b>
        with different types of tools such as <b>Docker</b>, and <b>Kubernetes</b> just to name a few. Moreover,
        I've deployed a number of websites with some major cloud service providers such as <b>Google Cloud</b>,
        <b>AWS</b>, and <b>DigitalOcean</b>.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Skills",
  data() {
    return {
      skillImages: [
        'html',
        'css',
        'javascript',
        'nodejs',
        'reactjs',
        'vuejs',
        'graphql',
        'php',
        'wordpress',
        'laravel',
        'nginx',
        'docker',
        'kubernetes',
      ]
    }
  },
  methods: {
    getIconUrl(name) {
      return require(`@/assets/icons/${name}.png`)
    }
  }
}
</script>

<style lang="scss" scoped>
@media screen and (max-width: 768px) {
  .skills {
    flex-direction: column-reverse;
    display: flex;
    .icons-wrapper {
      text-align: center;
      .icon-skill {
        display: inline-block;
      }
    }
  }
}
</style>
