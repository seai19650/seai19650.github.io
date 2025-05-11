<template>
  <section class="section">
    <div class="container">
      <div class="columns">
        <div class="column">
          <h3 class="title is-3">Showcases</h3>
        </div>
      </div>
    </div>

    <div class="highlights mt-6">
      <Highlight
          v-for="(item, index) in items"
          :key="index"
          :title="item.title"
          :description="item.description"
          :action="item.action"
          :images="item.images"
          :labels="item.labels"
      >
      </Highlight>
    </div>
  </section>
</template>

<script>
import Highlight from "@/Highlight";
export default {
  name: "Showcase",
  components: {Highlight},
  data () {
    return {
      items: [
        {
          title: 'Faculty of Information Technology',
          description: 'Building up the new website for the Faculty of Information Technology. Based on WordPress: Powerful Content Management System. Customize it with a custom WordPress theme designed only for this project.',
          action: {
            link: 'https://www.it.kmitl.ac.th',
            text: 'Go to website',
          },
          images: [
            'web-it-kmitl.png',
          ],
          labels: ['Website Development']
        },
        {
          title: 'A Scalable Service Architecture for Resource-Intensive Tasks',
          description: 'A RESTful-API that handles resource-intensive tasks by pushing tasks to machine learning node(s) intelligently and adequately scale depends on the load—currently used by Thailand Science Research and Innovation.',
          action: {
            link: 'https://ieeexplore.ieee.org/document/9158114',
            text: 'Go to IEEE Xplore Paper',
          },
          images: ['web-queueing.jpeg'],
          labels: ['Publication', 'IEEE Xplore']
        },
        {
          title: 'Registration System for TEDxKMITL',
          description: 'Implemented a web application that handles user registration and behind the scene process.',
          action: {
            link: 'https://blog.zartre.com/tedxkmitl2017-1e92f18687de',
            text: 'Go to Medium (Thai Language)',
          },
          images: ['web-tedxkmitl.png'],
          labels: ['TEDx', 'Website Development']
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
  .highlights {
    gap: 3rem;
    display: flex;
    flex-direction: column;
  }
</style>