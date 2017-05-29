<template>
    <div id="app">
        <datatable :setup="setup"></datatable>
    </div>
</template>

<script>
  import Datatable from './components/Datatable.vue'

  export default {
    name: 'app',
    data () {
      return {
        setup: {
          columns: [
            'id', 'title', 'description', 'date'
          ],
          sort: {
            column: 'id',
            direction: 'desc'
          },
          filter: 'category'
        }
      }
    },
    components: {
      Datatable
    }
  }
</script>
