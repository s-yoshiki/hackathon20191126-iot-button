<template>
  <div class="home">
    <Index msg=""/>
  </div>
</template>

<script>
// @ is an alias to /src
import Index from '@/components/Index.vue'

export default {
  name: 'index',
  components: {
    Index
  }
}
</script>
