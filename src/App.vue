<template>
  <div id="app" class="container pt-20 flex justify-center">
    <Chat />
  </div>
</template>

<script>
import Chat from './components/Chat';

export default {
  name: 'App',
  components: {
    Chat,
  }
}
</script>

<style src="./assets/tailwind.css">