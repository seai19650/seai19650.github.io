<template>
  <div>
    <div class="bg-cover do-center full-viewport-height">
      <div id="header" class="full-height do-center is-block-mobile p-5">
        <figure class="image is-hidden-mobile is-250x250">
          <img loading="lazy" class="is-rounded" src="@/assets/me.jpg">
        </figure>
        <div class="cover-content has-text-white">
          <div class="columns is-flex-mobile is-hidden-tablet is-centered">
            <figure class="column image is-flex-mobile is-hidden-tablet">
              <img loading="lazy" class="is-rounded" src="@/assets/me.jpg">
            </figure>
            <div class="column is-11">
              <h3 class="is-size-3">Kasidis Chaowvasin</h3>
            </div>
          </div>
          <h1 class="is-size-1 is-hidden-mobile">Kasidis Chaowvasin</h1>
          <div class="is-divider" data-content="Portfolio"></div>
            <p id="portfolio-description" class="mb-4">
              <span :class="['is-block mt-1', { ['box']: !typingFinished }]"><span class="typing-text">{{ myName }}</span><span class="cursor" v-if="!typingFinished" :class="{ 'blink': cursorBlink }">|</span>, a full-stack developer who built awesome web and mobile apps.</span>
              <span class="is-block mt-1">I'm a pro at front-end stuff like React and Vue.js, backend development with languages like GoLang, SpringBoot, Node.js, and PHP, and mobile development using React Native.</span>
              <span class="is-block mt-1">I'm all about making sure software is built right. I do lots of testing with tools like Cypress, Appium, and Detox to make sure everything works smoothly and reliably. Deploy and deliver with confidence.</span>
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
      <div class="scroll-down-container">
        <div class="arrow-down bounce">
          <i class="fas fa-chevron-down"></i>
        </div>
      </div>
    </div>

    <div class="container">

      <!-- showcases section-->
      <Showcase/>

      <!-- experience section-->
      <div class="is-divider"></div>
      <Experience/>

      <!--skill section-->
      <!-- <Skill/> -->

      <!-- chat section -->
      <div class="is-divider"></div>
      <ChatWithMe/>

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
// import Skill from "@/sections/Skill";
import Showcase from "@/sections/Showcase";
import Experience from "@/sections/Experience";
import Hobby from "@/sections/Hobby";
import ContactLink from "@/components/ContactLink";

export default {
  name: "App",
  components: {ContactLink, Experience, Showcase, Hobby},
  data () {
    return {
      myName: "I'm Kasidis Chaowvasin",
      myAnotherName: "Or you can call me Dem",
      cursorBlink: true,
      typingFinished: true,
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
  },
  mounted() {
    // Start the typing animation
    setTimeout(() => {
      this.typingFinished = false;
      this.typeWriter();
    }, 1000);
    this.cursorBlinkInterval = setInterval(() => {
      this.cursorBlink = !this.cursorBlink;
    }, 500);
  },
  beforeUnmount() {
    clearInterval(this.cursorBlinkInterval);
  },
  methods: {
    typeWriter() {
      // Stop cursor from blinking during deletion
      this.cursorBlink = false;
      
      // If myName still has characters, remove one
      if (this.myName.length > 0) {
        this.myName = this.myName.slice(0, -1);
        setTimeout(this.typeWriter, Math.floor(Math.random() * 5) + 50);
      } 
      // Once myName is empty, start adding characters from myAnotherName
      else {
        // Resume cursor blinking
        this.cursorBlink = true;
        this.typewriterAddChars(0);
      }
    },
    typewriterAddChars(index) {
      // Pause cursor blinking during character addition
      this.cursorBlink = false;
      
      // If we haven't added all characters from myAnotherName yet
      if (index < this.myAnotherName.length) {
        this.myName += this.myAnotherName.charAt(index);
        index++;
        setTimeout(() => {
          // Resume cursor blinking between characters
          this.cursorBlink = true;
          setTimeout(() => this.typewriterAddChars(index), Math.floor(Math.random() * 2) + 50);
        }, 30);
      } else {
        setTimeout(() => {
          // Resume cursor blinking when done typing
          this.cursorBlink = true;
          this.typingFinished = true;
        }, 1000);
      }
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

  #portfolio-description span {
    transition: all 0.1s;
  }

  .bg-cover {
    width: 100%;
    min-height: 65vh;
    background-color: #1F2833;
    overflow: hidden;
    position: relative;

    .cover-content {
      z-index: 1;
      max-width: 500px;

      h1 {
        z-index: inherit;
      }
    }
  }

  .full-viewport-height {
    min-height: 80vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .scroll-down-container {
    position: absolute;
    bottom: 30px;
    width: 100%;
    text-align: center;
  }

  .arrow-down {
    color: white;
    font-size: 2rem;
    cursor: pointer;
  }

  .bounce {
    animation: bounce 2s infinite;
  }

  @keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
      transform: translateY(0);
    }
    40% {
      transform: translateY(-20px);
    }
    60% {
      transform: translateY(-10px);
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

  .typing-text {
    display: inline;
  }
  
  .cursor {
    display: inline-block;
    font-weight: bold;
    margin-left: 1px;
    position: relative;
    color: #1F2833;
  }
  
  .cursor.blink {
    opacity: 1;
    animation: blink 1s step-end infinite;
  }
  
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }
}
</style>
