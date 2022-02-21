<template>
  <h1 class="success" v-if="this.$route.params.success">Message sent successfully ✓</h1>
  <h1>Message history</h1>
  <div v-if="allMessages.length">
    <div v-for="message in allMessages" :key="message.id">
      <SingleMessage :message="message" />
    </div>
  </div>
  <div v-else>
    <p class="note">There are no messages. Maybe send some? ✉️</p>
  </div>
</template>

<script>

import SingleMessage from '../components/SingleMessage.vue'
export default {
  components: { SingleMessage },
  data() {
    return {
      allMessages: []
    }
  },
  mounted() {
    this.allMessages = JSON.parse(localStorage.getItem("messages")) || []
  }
}
</script>

<style>
  .note {
    text-align: center;
  }
</style>