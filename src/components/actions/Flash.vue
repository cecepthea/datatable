<template>
    <div class="message">{{ message }}</div>
</template>
<script>
  export default {
    name: 'flash',
    props: [
      'message'
    ]
  }
</script>
<style scoped>
    .message {
        position: absolute;
        display: block;
        top: 50%;
        left: 50%;
        border: 1px rgba(193, 193, 193, 0.27) solid;
        padding: 10px 20px;
        z-index: 99999;
        background-color: #F8F;
        box-shadow: rgba(40, 63, 82, 0.7) 2px 2px 10px 0;
    }
</style>