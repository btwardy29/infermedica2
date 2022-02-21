<template>
  <div class="home">
    <h1>Send a new message</h1>
    <Form />
  </div>
</template>

<script>
// @ is an alias to /src
import Form from '@/components/Form.vue'

export default {
  name: 'Home',
  components: {
    Form,
  }
}
</script>
