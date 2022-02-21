<template>
  <form @submit.prevent="handleSubmit">
    <!-- Title input -->
    <label class="inputLabel">Title</label>
    <input 
      type="text" 
      v-model="title"
      placeholder="Enter the title" 
      :class="`inputField ${errors.titleError && `errorInput`}`" 
    />
    <div class="errorWrapper"><p v-if="errors.titleError" class="errorTitle">{{ errors.titleError }}</p></div>
    <!-- Message input -->
    <label class="inputLabel">Message</label>
    <textarea 
      v-model="content" 
      placeholder="Enter the message here..."
      :class="`inputField ${errors.contentError && `errorInput`}`" 
    />
    <div class="errorWrapper"><p v-if="errors.contentError" class="errorTitle">{{ errors.contentError }}</p></div>
    <!-- Character selection -->
    <label class="inputLabel">Character</label>
    <label class="select" for="slct">
    <select required="required" v-model="character">
      <option :value="null" disabled selected>Select character</option>
      <option v-for="character in characters" :key="character.id" :value="character">{{ character.name }}</option>
    </select>
    <div class="errorWrapper"><p v-if="errors.characterError" class="errorTitle">{{ errors.characterError }}</p></div>
    </label>
    <!-- InterGalaxy Quickpost checkbox -->
    <div class="checkboxContainer">
      <label>
        <input type="checkbox" checked="checked" v-model="quickpost" @click="()=>quickpost = !quickpost">
        <span class="checkmark"></span>
        <span class="checkboxDescription">I want to use InterGalaxy Quickpost™</span>
        <div class="errorWrapper"><p v-if="errors.quickpostError" class="errorTitle">{{ errors.quickpostError }}</p></div>
      </label>
    </div>
    <!-- Submit btn -->
    <button type="submit">Send</button>
  </form>
</template>

<script>
import { ref } from "vue";
import { useRouter } from 'vue-router'
import { nanoid } from 'nanoid'

export default {
  setup() {
    const router = useRouter()
    const title = ref('')
    const content = ref('')
    const character = ref(null)
    const quickpost = ref(false)
    const errors = ref({
      contentError: '',
      titleError: '',
      quickpostError: '',
      characterError: ''
    })
    return {
      title,
      content,
      character,
      quickpost,
      errors,
      router
    }
  },

  data() {
    return {
      characters: [],
      allMessages: [],
    }
  },

  methods: {
    handleSubmit() {
      // Clear previous errors
      this.errors.titleError = ''
      this.errors.contentError = ''
      this.errors.quickpostError = ''
      this.errors.characterError = ''
      // Regular Expression title condition
      const titleRegex = /^[a-zA-Z0-9 ]{3,32}$/
      const titleMatch = titleRegex.test(this.title)
      // Message length condition
      const contentMatch = !!this.content.length && this.content.length < 256
      // Handle errors
      if(!titleMatch) {
        this.errors.titleError = 'Incorrect title (3-32 characters long and has no special characters).'
      }
      if(!contentMatch) {
        this.errors.contentError = 'Message is required and must be shorter than 256 characters.'
      }
      if(!this.quickpost) {
        this.errors.quickpostError = 'You must accept terms of InterGalaxy Quickpost.'
      }
      if(!this.character) {
        this.errors.characterError = 'You must pick a character you want to send message to.'
      }
      // Create timestamp
      const timestamp = new Date()
      // Create message object
      const fullMessage = {
        // Create unique ID with nanoid 
        id: nanoid(),
        title: this.title,
        content: this.content,
        character: this.character,
        createdAt: timestamp,
      }
      // Push message into array and redirect if there's no errors
      if(titleMatch && contentMatch && this.quickpost && this.character) {
        this.allMessages = [...this.allMessages, fullMessage]
        this.router.push({ name: 'History', params: { success: 'success' } })
      }
    }
  },
// Watch for changes in allMessages array
 watch: {
   allMessages(newValue) {
      localStorage.setItem("messages", JSON.stringify(newValue))
   }
 },

  mounted() {
    fetch('https://rickandmortyapi.com/api/character')
      .then(res => res.json())
      .then(({ results }) => this.characters = results)
    // Initial fetching messages from local storage
    this.allMessages = JSON.parse(localStorage.getItem("messages")) || []
  },
  name: "Form",
  props: {
  },
}
</script>

<style scoped lang="scss">
@import '../assets/variables.scss';
// OPTIONS MENU
.select {
	position: relative;
	min-width: 200px;
  outline: none;
	select {
    outline: none;
		-webkit-appearance: none;
		padding: 12px 16px;
		width: 100%;
		border: 1px solid #e8eaed;
		border-radius: 8px;
		cursor: pointer;
		font-family: inherit;
		font-size: 14px;
		&:required {
			&:invalid {
				color: #5a667f;
			}
		}

		option[value=""][disabled] {
			display: none;
		}
	}
}


.checkboxContainer {
  margin-top: 50px;
  label {
    font-size: 20px;
  }
}
.checkboxDescription {
  font-weight: 600;
  font-size: 16px;
}
/* Customize the label (the container) */
.checkboxContainer {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.checkboxContainer input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  border: 1px solid #D6DBE4;
  border-radius: 4px;
  cursor: pointer;
}


/* When the checkbox is checked, add a blue background */
.checkboxContainer input:checked ~ .checkmark {
  background-color: #4EADC5;
  border-radius: 4px;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.checkboxContainer input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.checkboxContainer .checkmark:after {
  content: "✓";
  color: white;
  transform: translateX(4px);
}

.inputLabel  {
  margin: 12px 0;
}

.inputField {
  border: 1px solid $primary-gray;
  border-radius: 8px;
  padding: 10px 16px;
  font-size: 14px;
  outline: none;
  width: 100%;
  display: block;
}


</style>