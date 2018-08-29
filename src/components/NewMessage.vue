<template>
  <div class="new-message">
    <form @submit.prevent="addMessage">
      <label for="new-message">New Message (enter to add): </label>
      <div style="display: flex; align-items: flex-end; flex-direction: row">
        <input type="text" name="new-message" v-model="newMessage">
        <i @click="addMessage" id="send-icon" style="margin-left: 1em" class="material-icons">send</i>
      </div>
      <p class="red-text" v-if="feedback">{{ feedback }}</p>
    </form>
  </div>
</template>

<script>
import db from '@/firebase/init'

export default {
  name: 'NewMessage',
  props: ['name'],
  data() {
    return {
      newMessage: null,
      feedback: null
    }
  },
  methods: {
    addMessage() {
      if(this.newMessage) {
        db.collection('messages').add({
          content: this.newMessage,
          name: this.name,
          timestamp: Date.now()
        }).catch(err => {
          console.log(err)
        });
        this.newMessage = null;
        this.feedback = null
      } else {
        this.feedback = "Enter message first"
      }
    }
  }
}
</script>

<style>
  #send-icon {
    cursor: pointer !important;
  }

  #send-icon:hover{
    color: #009688 !important
  }
</style>
