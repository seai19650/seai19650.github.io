<template>
  <section class="section">
    <div class="container">
      <div class="columns is-multiline">
        <div class="column is-12">
          <div class="text-able">
            <Label text="Hobby"/>
            <h3 class="title is-3 has-text-white">Scuba Diving</h3>
            <p class="has-text-white mb-1">
              I'm a certified open water diver and plan to earn my advanced license soon. I love being underwater—it's calm, relaxing, and beautiful. There's so much to discover beneath the surface.
            </p>
            <p class="has-text-white">
              Scuba diving not only lets me explore the underwater world and meet new people, but it also teaches me to stay calm, manage fear, and focus on what's happening right in front of me.
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Label from "@/components/Label";
export default {
  name: 'Hobby',
  components: {Label},
}
</script>

<style lang="scss" scoped>
  .section {
    background: url('../assets/hobbies/scuba-diving.jpeg') no-repeat top;
    background-size: cover;
    .container {
      min-height: 700px;
      .text-able {
        @media screen and (min-width: 710px) {
          width: 50%;
        }
        @media screen and (min-width: 600px) and (max-width: 709px) {
          width: 60%;
        }
      }
    }
  }
</style>
