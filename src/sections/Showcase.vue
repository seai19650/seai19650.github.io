<template>
  <h3 class="title is-3">Showcases</h3>

  <Highlight
      v-for="(item, index) in items"
      :key="index"
      :title="item.title"
      :description="item.description"
      :action="item.action"
      :image="item.image"
      :labels="item.labels"
  >
  </Highlight>

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
          description: 'Building up the new website for Faculty of Information Technology. Based on Wordpress: Powerful Content Management System. Customized with custom Wordpress theme designed only for this project.',
          action: {
            link: 'https://www.it.kmitl.ac.th',
            text: 'Go to website',
          },
          image: 'web-it-kmitl.png',
          labels: ['Website Development']
        },
        {
          title: 'A Scalable Service Architecture for Resource-Intensive Tasks',
          description: 'A RESTful-API that handle tasks from users, push tasks to machine learning node(s) in the proper way and intelligently scale depends on load',
          action: {
            link: 'https://ieeexplore.ieee.org/document/9158114',
            text: 'Go to IEEE Xplore Paper',
          },
          image: 'web-queueing.jpeg',
          labels: ['Publication', 'IEEE Xplore']
        },
        {
          title: 'Registration System for TEDxKMITL',
          description: 'Implementing a web application that handle user registration and behind the scene process.',
          action: {
            link: 'https://blog.zartre.com/tedxkmitl2017-1e92f18687de',
            text: 'Go to Medium (Thai Language)',
          },
          image: 'web-tedxkmitl.png',
          labels: ['TEDx', 'Website Development']
        }
      ]
    }
  }
}
</script>

<style scoped>

</style>
