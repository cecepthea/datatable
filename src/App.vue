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
          per_page: 5,
          columns: [
            {
              name: 'id',
              title: '#ID',
              use: 'data-text'
            },
            {
              name: 'last_name',
              title: 'Last Name',
              use: 'data-text'
            },
            {
              name: 'first_name',
              title: 'First Name',
              use: 'data-text'
            },
            {
              name: 'avatar',
              title: 'Avatar',
              use: 'data-image'
            }
          ],
          sort: {
            column: 'last_name',
            direction: 'desc'
          },
          ajax: {
            root: 'https://reqres.in',
            url: '/api/users',
            method: 'GET'
          },
          filters: [
            {
              field: 'title',
              values: ['one', 'two', 'three'],
              selected: null
            }
          ],
          actions: [
            {
              title: 'view',
              style: 'btn-default',
              content: '<i class="glyphicon glyphicon-zoom-in"></i>'
            },
            {
              title: 'edit',
              style: 'btn-default',
              content: '<i class="glyphicon glyphicon-edit"></i>'
            },
            {
              title: 'approve',
              style: 'btn-default',
              content: '<i class="glyphicon glyphicon-thumbs-up"></i>'
            },
            {
              title: 'decline',
              style: 'btn-default',
              content: '<i class="glyphicon glyphicon-thumbs-down"></i>'
            },
            {
              title: 'delete',
              style: 'btn-default',
              content: '<i class="glyphicon glyphicon-trash"></i>'
            }
          ]
        }
      }
    },
    components: {
      Datatable
    }
  }
</script>
