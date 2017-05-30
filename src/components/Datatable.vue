<template>
    <div>
        <table class="table table-striped table-bordered">
            <thead>
            <tr>
                <th :colspan="config.columns.length+1">
                    <div class="col-xs-3">
                        <div class="input-group">
                            <span class="input-group-addon" id="basic-addon">
                                <i class="glyphicon glyphicon-search"></i>
                            </span>
                            <input
                                    type="text"
                                    class="form-control"
                                    placeholder="Username"
                                    @click="getData"
                                    v-model="config.search"
                                    aria-describedby="basic-addon">
                        </div>
                    </div>
                </th>
            </tr>
            <tr class="table-header">
                <th
                        v-for="column in config.columns"
                        @click="sortDirection(column)"
                        class="clickable"
                        :class="{'active-header-column': column.name == config.sort.column}">
                    <span :class="{'text-info': column.name == config.sort.column}">{{ column.title }}</span>
                    <span v-if="column.name == config.sort.column" class="pull-right">
                            <i class="glyphicon glyphicon-chevron-down" v-if="config.sort.direction == 'desc'"></i>
                            <i class="glyphicon glyphicon-chevron-up" v-else></i>
                    </span>
                </th>
                <th v-if="config.actions.length > 0">
                    Actions
                </th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="row in result">
                <td v-for="column in config.columns"
                    :class="{'active-column text-muted': column.name == config.sort.column}">
                    <component :is="column.use" :content="row[column.name]"></component>
                </td>
                <td v-if="config.actions.length > 0" width="15%">
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
        <flash :message="message.title" v-if="message"></flash>
    </div>
</template>
<script>
  import axios from 'axios'
  import DataImage from './data/DataImage.vue'
  import DataText from './data/DataText.vue'
  import Flash from './actions/Flash.vue'

  export default {
    name: 'datatable',
    props: [
      'setup'
    ],
    components: {
      DataText,
      DataImage,
      Flash
    },
    data () {
      return {
        config: {
          ajax: {},
          search: '',
          columns: [],
          sort: {
            column: 'id',
            direction: 'desc'
          },
          actions: []
        },
        result: [],
        message: false
      }
    },
    methods: {
      sortDirection (column) {
        if (this.config.sort.column !== column.name) {
          this.config.sort = {column: column.name, direction: 'desc'}
          return
        }
        this.config.sort.direction = this.config.sort.direction === 'asc' ? 'desc' : 'asc'
      },
      actionEvent (id, action) {
        this.message = action
        console.log(id, action)
      },
      getData () {
        let vm = this
        axios({
          url: vm.config.ajax.root + vm.config.ajax.url,
          method: vm.config.ajax.method,
          params: vm.query
        })
          .then(function (response) {
            let result = response.data
            vm.result = result.data
          })
      }
    },
    computed: {
      query () {
        const conf = this.config
        return {
          search: conf.search,
          sort: conf.sort.column,
          direction: conf.sort.direction,
          per_page: conf.per_page
        }
      }
    },
    watch: {
      query (curr, prev) {
        if (curr.search.length > 3 || prev.search.length === curr.search.length) {
          this.getData()
        }
      }
    },
    beforeUpdate () {
      // console.log(this.query)
    },
    created () {
      for (let setting in this.setup) {
        if (this.setup.hasOwnProperty(setting)) {
          this.config[setting] = this.setup[setting]
        }
      }
      this.getData()
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

    .table-header {
        background-color: #e8e8e8;
    }

    .active-column {
        background-color: #e7f8e2;
    }

    .active-header-column {
        background-color: #c8c8c8;
    }
</style>