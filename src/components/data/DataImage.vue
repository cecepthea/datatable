<template>
    <img :src="content" class="img-circle"/>
</template>
<script>
  export default {
    name: 'data-image',
    props: [
      'content'
    ]
  }
</script>
<style>

</style>