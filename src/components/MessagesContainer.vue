<template>
  <ul>
    <li v-for="message in messageList" :key="message.id">
      <subot-message-item :isAgent="message.isAgent" :message="message.message"></subot-message-item>
    </li>
  </ul>
</template>

<script>
import MessageItem from './MessageItem.vue'

export default {
    components: {
        'subot-message-item': MessageItem
    },
    props: ['messageList'],
};
</script>

<style>
</style>