<template>
  <section class="section">
    <h3 class="title is-3">Skills</h3>
    <div class="skills-section box">
      <template v-for="(category, categoryName, index) in skillCategories" :key="categoryName">
        <!-- Add divider before all categories except the first -->
        <hr v-if="index > 0" class="skill-divider my-6">
        
        <div class="skill-category mb-6">
          <div class="columns is-multiline">
            <div class="column is-7-desktop">
              <h4 class="title is-4">{{ categoryName }}</h4>
              <p class="mb-3" v-html="category.description"></p>
            </div>
            <div class="column is-5-desktop">
              <div class="skill-icons-outer-container">
                <div class="skill-icons-container">
                  <div 
                    v-for="(skillName, skillIndex) in displayedSkills(category.skills)" 
                    :key="skillIndex" 
                    class="skill-icon-item"
                    :style="{ 
                      zIndex: displayedSkills(category.skills).length - skillIndex,
                      transform: `translateX(-${skillIndex * 15}px)`
                    }"
                  >
                    <div 
                      class="skill-icon-wrapper"
                      :style="{ 
                        width: `${100 - skillIndex * 6}px`, 
                        height: `${100 - skillIndex * 6}px` 
                      }"
                    >
                      <img v-if="iconExists(skillName)" 
                          loading="lazy" 
                          :src="getIconUrl(skillName)" 
                          :alt="skillName">
                      <div v-else class="skill-name">{{ skillName }}</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </div>
  </section>
</template>

<script>
export default {
  name: "Skills",
  data() {
    return {
      availableIcons: null,
      maxVisibleIcons: 6, // Maximum number of icons to display in a row
      skillCategories: {
        "Frontend Development": {
          skills: ['html', 'css', 'javascript', 'reactjs', 'vuejs'],
          description: 'My core frontend toolkit includes <b>HTML</b>, <b>CSS</b> (including <b>SCSS</b>), and <b>JavaScript</b>. I\'ve worked with various CSS frameworks like <b>Bootstrap</b>, <b>Bulma</b>, and <b>Foundation</b>, making the <b>grid system</b> second nature to me. For JavaScript frameworks, I specialize in <b>Vue.js</b> and <b>React.js</b>, having built numerous applications with them.'
        },
        "Backend Development": {
          skills: ['nodejs', 'php', 'wordpress', 'laravel', 'graphql'],
          description: 'For backend development, I primarily use <b>JavaScript</b> with <b>Node.js</b> and <b>Express.js</b>. I\'m also comfortable with <b>PHP</b> and its popular frameworks like <b>WordPress</b> and <b>Laravel</b>. When it comes to databases, I can write and optimize <b>SQL</b> queries to enhance performance. I\'ve also worked with <b>GraphQL</b> for efficient API development.'
        },
        "Testing": {
          skills: ['cypress', 'cucumber', 'selenium'],
          description: 'I have experience with various testing tools and methodologies. I use <b>Cypress</b> and <b>Selenium</b> for end-to-end testing, <b>Cucumber</b> for behavior-driven development, and <b>Jest</b> for unit testing. I\'m comfortable implementing test-driven development practices and creating comprehensive test suites to ensure application quality and reliability.'
        },
        "DevOps": {
          skills: ['azure','terraform','helm', 'docker', 'kubernetes', 'nginx','aws', 'gcp'],
          description: 'I have experience in <b>Linux server administration</b> and web service <b>deployment</b> using tools like <b>Docker</b>, <b>Kubernetes</b>, and <b>Helm</b>. I\'ve deployed several websites on major cloud platforms such as <b>Google Cloud</b>, <b>AWS</b>, and <b>Azure</b>. I\'m familiar with infrastructure as code through <b>Terraform</b>, CI/CD pipelines, and automation strategies to streamline development and deployment workflows.'
        }
      }
    }
  },
  created() {
    // Initialize the list of available icons
    this.initAvailableIcons();
  },
  methods: {
    initAvailableIcons() {
      try {
        // This will get all the icon files
        const iconContext = require.context('@/assets/icons', false, /\.png$/);
        this.availableIcons = iconContext.keys().map(key => {
          // Extract filename without extension
          return key.split('/').pop().replace('.png', '');
        });
      } catch (error) {
        console.warn('Error loading icons:', error);
        this.availableIcons = [];
      }
    },
    iconExists(name) {
      if (!this.availableIcons) {
        return false;
      }
      return this.availableIcons.includes(name);
    },
    getIconUrl(name) {
      try {
        return require(`@/assets/icons/${name}.png`);
      } catch (error) {
        console.warn(`Icon not found for ${name}`);
        return '';
      }
    },
    // Method to limit the number of displayed skills to prevent parent overflow
    displayedSkills(skills) {
      return skills.slice(0, this.maxVisibleIcons);
    }
  }
}
</script>

<style lang="scss" scoped>
.skill-category {
  &:nth-child(even) {
    .columns {
      flex-direction: row-reverse;
    }
  }
}

@media screen and (max-width: 768px) {
  .skill-category .columns {
    flex-direction: column-reverse;
    display: flex;
  }
}

.skill-icons-outer-container {
  width: 100%;
  overflow: hidden; // Ensure nothing extends beyond this container
  padding: 10px 0;
  position: relative;
}

.skill-icons-container {
  display: flex;
  align-items: center;
  position: relative;
  padding: 10px 0;
  margin-left: 10px; // Reduced margin to ensure icons stay within container
  width: calc(100% - 20px); // Adjust width to account for margins
}

.skill-icon-item {
  flex-shrink: 0;
  
  &:hover .skill-icon-wrapper {
    transform: scale(1.15);
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
    z-index: 100;
  }
}

.skill-icon-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  border-radius: 50%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: all 0.3s ease;
  position: relative;
  
  img {
    max-width: 75%;
    max-height: 75%;
    object-fit: contain;
  }
}

.skill-name {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: capitalize;
  font-weight: bold;
  font-size: 1rem;
  color: #555;
}

.skill-divider {
  height: 1px;
  background: linear-gradient(to right, transparent, rgba(0,0,0,0.15), transparent);
  border: none;
  max-width: 80%;
  margin-left: auto;
  margin-right: auto;
}

@media screen and (max-width: 768px) {
  .skill-icons-container {
    margin-left: 15px;
  }
}

@media screen and (max-width: 480px) {
  .skill-icon-wrapper {
    width: 80px !important;
    height: 80px !important;
  }
  
  .skill-icon-item {
    transform: translateX(-10px) !important;
  }
}
</style>
