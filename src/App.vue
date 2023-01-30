<template>

  <div class="container">
    <discussions DiscussionTemplateJson="true"></discussions>
  </div>

</template>

<script>
import Discussions from './components/modules/discussion/DiscussionsIndex.vue'

export default {
  name: 'App',
  components: {
    Discussions
  }
}
</script>

