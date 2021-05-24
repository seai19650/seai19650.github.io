<template>
  <div>
    <div class="bg-cover mb-5 do-center">
      <div class="full-height do-center">
        <figure class="image is-300x300 mr-5">
          <img class="is-rounded" src="@/assets/me.jpg">
        </figure>
        <div class="cover-content has-text-white">
          <h1 class="is-size-1">Kasidis Chaowvasin</h1>
          <div class="is-divider" data-content="Portfolio"></div>
          <p class="mb-4">Hello, I'm Dem. A full-stack web developer. I now mainly work with JavaScript and related technologies - but also comfortable with PHP (Wordpress and Laravel are awesome!)
            I love trying new things, sharing knowledge, inspiring people, and getting inspired too :)</p>
            <ContactLink
                class="mb-1"
                v-for="contact in contacts"
                :key="contact.name"
                :name="contact.name"
                :icon="contact.icon"
                :link="contact.link"
                :text="contact.text"
            >
            </ContactLink>
        </div>
      </div>
    </div>

    <div class="container">

      <!--skill section-->
      <div class="box">
        <Skill/>
      </div>

      <!-- showcases section-->
      <Showcase/>

      <!-- experience section-->
      <div class="is-divider"></div>
      <Experience/>

    </div>

    <footer class="footer">
      <div class="columns is-multiline">
        <div class="column is-narrow">
          <p class="has-text-weight-bold has-text-white">Contact</p>
        </div>
        <div class="is-divider-vertical"></div>
        <div class="column is-12-mobile is-4-desktop">
          <ContactLink
              class="mb-1"
              v-for="contact in contacts"
              :key="contact.name"
              :name="contact.name"
              :icon="contact.icon"
              :link="contact.link"
              :text="contact.text"
          >
          </ContactLink>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import Skill from "@/sections/Skill";
import Showcase from "@/sections/Showcase";
import Experience from "@/sections/Experience";
import ContactLink from "@/components/ContactLink";

export default {
  name: "App",
  components: {ContactLink, Experience, Showcase, Skill},
  data () {
    return {
      contacts: [
        {
          name: 'GitHub',
          icon: 'github',
          link: 'https://github.com/seai19650',
        },
        {
          name: 'LinkedIn',
          icon: 'linkedin',
          link: 'https://www.linkedin.com/in/kasidisc/',
        },
        {
          name: 'Phone',
          icon: 'phone',
          text: '+66 63 313 7902',
        },
        {
          name: 'Email',
          icon: 'email',
          text: 'seai_kasidis@hotmail.com',
        }
      ]
    }
  }
};
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  .bg-cover {
    width: 100%;
    min-height: 65vh;
    background-color: #1F2833;
    overflow: hidden;

    .cover-content {
      z-index: 1;
      max-width: 500px;

      h1 {
        z-index: inherit;
      }
    }
  }

  footer {
    background-color: #1F2833;
  }
}
</style>
