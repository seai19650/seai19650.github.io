<template>
  <div>
    <div class="bg-cover mb-5 do-center">
      <div id="header" class="full-height do-center is-block-mobile p-5">
        <figure class="image is-250x250">
          <img loading="lazy" class="is-rounded" src="@/assets/me.jpg">
        </figure>
        <div class="cover-content has-text-white">
          <h1 class="is-size-1">Kasidis Chaowvasin</h1>
          <div class="is-divider" data-content="Portfolio"></div>
          <p class="mb-4">Hello, I'm Dem. A full-stack web developer. I am now mainly working with JavaScript and related technologies. I am also comfortable with PHP (WordPress and Laravel are fantastic!) I love trying new things, sharing ideas, inspiring people, getting inspired, and meeting new people.
</p>
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

    <!-- hobby section-->
    <Hobby/>

    <footer class="footer do-center">
      <div class="columns is-multiline">
        <div class="column is-narrow">
          <p class="has-text-weight-bold has-text-white">Contact</p>
        </div>
        <div class="is-divider-vertical"></div>
        <div class="column is-narrow">
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
      <small class="credit-icons has-text-white has-text-centered mt-4">
        with help : icons from
        <a href="https://icons8.com/" target="_blank">icons8</a> ·
        <a href="https://github.com/cypress-io/cypress-icons" target="_blank">cypress-io</a> ·
        <a href="https://cucumber.io/" target="_blank">cucumber</a> ❤️
      </small>
    </footer>
  </div>
</template>

<script>
import Skill from "@/sections/Skill";
import Showcase from "@/sections/Showcase";
import Experience from "@/sections/Experience";
import Hobby from "@/sections/Hobby";
import ContactLink from "@/components/ContactLink";

export default {
  name: "App",
  components: {ContactLink, Experience, Showcase, Skill, Hobby},
  data () {
    return {
      contacts: [
        {
          name: 'Email',
          icon: 'email',
          text: 'seai_kasidis@hotmail.com',
        },
        {
          name: 'LinkedIn',
          icon: 'linkedin',
          link: 'https://www.linkedin.com/in/kasidisc/',
        },
        {
          name: 'GitHub',
          icon: 'github',
          link: 'https://github.com/seai19650',
        },
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

  #header {
    figure {
      margin-right: 1.5rem;
    }
    @media screen and (max-width: 768px) {
      margin: 20px 0;
      text-align: center;
      figure {
        margin: 0 auto;
      }
    }
  }

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
    position: relative;
    background-color: #1F2833;
    @media screen and (max-width: 768px) {
      text-align: center;
    }
    .credit-icons {
      width: 100%;
      position: absolute;
      bottom: 0;
      margin-bottom: 10px;
    }
  }
}
</style>
