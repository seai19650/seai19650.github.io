<template>
  <section class="section">
    <div class="columns is-multiline">
      <div class="column is-narrow">
        <h3 class="title is-3">Experiences</h3>
        <div class="timeline">
          <div class="outer-timeline">
            <div class="inner-timeline is-justify-content-center">
              <div class="lines">
                <div class="dot"></div>
                <div class="line"></div>
                <div class="dot"></div>
              </div>
              <div class="cards">
                <div v-for="(experience, index) in experiences" :key="index" class="card">
                  <h4><Label v-for="label in experience.type" :key="label" :text="label"/>{{ experience.timeText }}</h4>
                  <p>{{ experience.title }}</p>
                  <div v-if="experience.description" class="is-divider my-3"/>
                  <p v-if="experience.description">{{ experience.description }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="column">
        <div id="photos">
          <div v-for="image in images" :key="image.path" class="card stack">
            <div class="card-image">
              <figure class="image is-4by3">
                <img loading="lazy" :src="getExperienceImg(image.path)" alt="Placeholder image">
              </figure>
            </div>
            <div class="card-content">
              <Label v-for="(label, index) in image.labels" :key="index" :text="label"/>
              <div class="content">
                {{ image.explain }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import 'vue3-carousel/dist/carousel.css'
import Label from "@/components/Label";

export default {
  name: "Experience",
  components: {
    Label,
  },
  data() {
    return {
      selected: 0,
      images: [
        {
          path: 'with-tim-cook',
          labels: ['Swift Coding Club Thailand'],
          explain: 'I had an excellent opportunity to talk with Tim Cook about Swift Coding Club Thailand that we\'re working on for a year. Teaching students how to build an app, share stories, and inspire them to do more. We strongly believe that this will lead us to a better future.'
        },
        {
          path: 'scc-1',
          labels: ['Swift Coding Club Thailand'],
          explain: 'Sharing knowledge and inspiration about coding with students at Saint Gabriel\'s College.'
        },
        {
          path: 'cimb',
          labels: ['Hackathon'],
          explain: 'Participated in CIMB Conquest (Hackathon) in Malaysia. In the topic "Application that applies new type on security and authentication."'
        },
        {
          path: 'japan',
          labels: ['Exchange Student'],
          explain: 'Had been selected to participate in an exchange program in Japan for a week. This program lets participants join a college of technology (Kosen) like an ordinary student. We had classes with Japanese students to share our culture. Taking and sharing many topics with Japanese students was terrific.'
        }
      ],
      experiences: [
        {
          type: ['Work'],
          timeText: 'Now',
          title: 'Junior Software Developer at PALO IT Thailand',
        },
        {
          type: ['Work'],
          timeText: 'June 2020 - August 2021',
          title: 'Software Developer at Fiveloop',
          description: 'Full-stack developer of web-based CRM using React, Apollo GraphQL, and backend service with Node.js.'
        },
        {
          type: ['Work'],
          timeText: '2019-2020',
          title: 'Software Developer at Montivory',
          description: 'Develop websites using Adobe Experience Manager.'
        },
        {
          type: ['Work'],
          timeText: 'June 2019 - August 2019',
          title: 'Backend Developer (Intern) at Morphosis',
          description: 'Be a part of an international developer team. Mainly worked on back-end.'
        },
        {
          type: ['Activity'],
          timeText: '2019 - 2020',
          title: 'Leader at Swift Coding Club Thailand',
          description: 'Organize activities/workshops for students who are interested in creating an iOS Application using Swift.'
        },
        {
          type: ['Activity'],
          timeText: '17 - 23 June 2018',
          title: 'Exchange Student at JST',
          description: 'Had an opportunity to be a Japanese student for a week!'
        }
      ]
    }
  },
  methods: {
    getExperienceImg(name) {
      return require(`@/assets/experiences/${name}.jpg`)
    }
  }
}
</script>

<style lang="scss" scoped>
#photos {
  -moz-column-count: 2;
  -webkit-column-count: 2;
  column-count: 2;
  gap: 12px;

  img {
    object-fit: cover;
  }

  .card.stack {
    overflow: hidden;
    -webkit-column-break-inside: avoid;
    page-break-inside: avoid;
    break-inside: avoid-column;
    display: inline-block;
    margin-bottom: 12px;
  }
  @media screen and (max-width: 950px) {
    -moz-column-count: 1;
    -webkit-column-count: 1;
    column-count: 1;
    gap: 6px;
  }
  @media screen and (max-width: 768px) {
    -moz-column-count: 2;
    -webkit-column-count: 2;
    column-count: 2;
    gap: 10px;
  }
  @media screen and (max-width: 710px) {
    -moz-column-count: 1;
    -webkit-column-count: 1;
    column-count: 1;
    gap: 10px;
  }
}

.timeline {
  .outer-timeline {
    margin-top: 99.5px;

    .inner-timeline {
      width: 100%;
      display: flex;

      .lines {
        @media screen and (max-width: 768px) {
          display: none;
        }
        margin-top: 6px;
        .dot {
          width: 14px;
          height: 14px;
          background: #D1D6E6;
          border-radius: 7px;
        }

        .line {
          height: 300px;
          width: 2px;
          background: #D1D6E6;
          margin-left: 5.3px;
        }
      }



      .cards {
        max-width: 310px;
        margin-left: 12px;
        transform: translateY(-50px);

        @media screen and (max-width: 768px) {
          max-width: 100%;
          margin-left: 0;
        }

        .card {
          padding: 20px 15px;
          border-radius: 10px;
          margin-bottom: 10px;

          h4 {
            font-style: normal;
            font-weight: bold;
            font-size: 14px;
            line-height: 19px;
            color: #2B2862;
          }

          p {
            font-style: normal;
            font-weight: normal;
            font-size: 16px;
            line-height: 22px;
            color: #2B2862;
          }
        }
      }
    }
  }
}
</style>
