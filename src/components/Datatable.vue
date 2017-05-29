<template>
    <table class="table table-stripped table-bordered">
        <thead>
        <tr>
            <th :colspan="config.columns.length+1">
                <div class="col-xs-3">
                    <div class="input-group">
                        <span class="input-group-addon" id="basic-addon">
                            <i class="glyphicon glyphicon-search"></i>
                        </span>
                        <input type="text" class="form-control" placeholder="Username" aria-describedby="basic-addon">
                    </div>
                </div>
            </th>
        </tr>
        <tr>
            <th v-for="column in config.columns" @click="sortDirection(column)" class="clickable">
                {{ column }}
                <span class="pull-right" v-if="column == config.sort.column">
                        <i class="glyphicon glyphicon-chevron-down" v-if="config.sort.direction == 'desc'"></i>
                        <i class="glyphicon glyphicon-chevron-up" v-else></i>
                    </span>
                <span class="pull-right" v-else>
                        <i class="glyphicon glyphicon-sort"></i>
                    </span>
            </th>
            <th v-if="config.actions.length>0">
                Actions
            </th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="row in result">
            <td v-for="column in config.columns">
                {{ row[column] }}
            </td>
            <td v-if="config.actions" width="15%">
                <div class="btn-group btn-group-sm" role="group">
                    <button
                            v-for="action in config.actions"
                            :class="action.style"
                            class="btn btn-sm"
                            @click="actionEvent(row.id, action)"
                            :title="action.title"
                            v-html="action.content == '' ? action.title : action.content">
                    </button>
                </div>
            </td>
        </tr>
        </tbody>
    </table>
</template>
<script>
  export default {
    name: 'datatable',
    props: [
      'setup'
    ],
    data () {
      return {
        config: {
          columns: [
            'id', 'title', 'description', 'date'
          ],
          sort: {
            column: 'id',
            direction: 'desc'
          },
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
        },
        result: [
          {
            id: 1,
            title: 'one',
            description: 'lorem ipsum',
            date: '2017-05-27'
          },
          {
            id: 2,
            title: 'two',
            description: 'sit dolor',
            date: '2017-05-28'
          }
        ]
      }
    },
    methods: {
      sortDirection (column) {
        console.log(this.config.sort)
        if (this.config.sort.column !== column) {
          this.config.sort = {column: column, direction: 'desc'}
          return
        }
        this.config.sort.direction = this.config.sort.direction === 'asc' ? 'desc' : 'asc'
      },
      actionEvent (id, action) {
        console.log(id, action)
      }
    },
    created () {
//      this.config = this.setup
//      console.log(this)
    }
  }
</script>
<style scoped>
    @import "/node_modules/bootstrap/dist/css/bootstrap.css";

    .clickable {
        cursor: pointer;
    }
</style>