<template>
  <div class="singleMessageWrapper">
    <div class="titleWrapper" @click="()=>showContent = !showContent">
      <span class="title">{{message.title}}</span>
      <span :class="`material-icons noRotation ${showContent && `rotate`}`">expand_more</span>
    </div>
      <div v-if="showContent">
      <div class="descriptionWrapper">
        <div class="avatar" :style="{ 'background-image': `url(${message.character.image})` }"/>
        <div class="description">
          <p>Sent to: {{ message.character.name }}</p>
          <p>Date: {{ this.getDate }}</p>
          <p>At: {{ this.getTime }}</p>
        </div>
        <span class="quickpostMark"><span class="material-icons success">done</span>Using Quickpost™</span>
      </div>
      <p class="content">{{message.content}}</p>
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
export default {
  props: ['message'],
  setup() {
    const showContent = ref(false)

    return {
      showContent
    }
  },
  computed: {
    getDate() {
      const date = new Date(this.message.createdAt)
      return date.toLocaleDateString(navigator.language)
    },
    getTime() {
      const date = new Date(this.message.createdAt)
      return date.toLocaleTimeString(navigator.language, { hour: '2-digit', minute:'2-digit' })
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';

.quickpostMark {
  display: none;
  align-items: center;
}
.singleMessageWrapper {
  border-bottom: 1px solid $primary-gray;
  color: #324B72;

}
.titleWrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}
.title {
  font-size: 14px;
  padding: 18px 0;
  font-weight: 600;
}
.content {
  padding: 20px 0;
  font-size: 16px;
}

.descriptionWrapper {
  display: flex;
  border-left: 1px solid $primary-gray;
  span {
    flex: 1;
    text-align: right;
  }
}
.avatar {
  min-height: 48px;
  max-height: 48px;
  aspect-ratio: 1;
  border-radius: 100px;
  background-position: center;
  background-size: cover;
  margin: 0 10px;
}
.rotate {
  transform: rotate(180deg) !important;
  transition: 0.5s;
}
.noRotation {
  transform: rotate(0deg);
  transition: 0.5s
}

@media screen and (max-width: 600px) {
  .quickpostMark {
    display: flex;
  }
}
</style>