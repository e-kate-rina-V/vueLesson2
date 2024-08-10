<template>
  <header>
    <div class="wrapper">
      <ReactiveComponent />
    </div>
  </header>
</template>

<script>
import ReactiveComponent from './components/ReactiveComponent.vue'

export default {
  name: 'App',
  components: {
    ReactiveComponent
  }
}
</script>
