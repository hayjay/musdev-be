<template>
  <b-row>
    <b-col cols="12" xl="6">
      <transition name="slide">
      <b-card>
        <div slot="header" v-html="caption"></div>
        <div class="row mr-auto ml-auto"> <h6>Treated</h6> <c-switch class="mx-1" color="primary" checked label outline /> <h6>Pending</h6></div>
        <b-table :hover="hover" :striped="striped" :bordered="bordered" :small="small" :fixed="fixed" responsive="sm" :items="items" :fields="fields" :current-page="currentPage" :per-page="perPage" @row-clicked="rowClicked">
          <template slot="id" slot-scope="data">
            <strong>{{data.item.id}}</strong>
          </template>
          <template slot="name" slot-scope="data">
            <strong>{{data.item.name}}</strong>
          </template>
          <template slot="status" slot-scope="data">
            <b-badge :variant="getBadge(data.item.status)">{{data.item.status}}</b-badge>
          </template>
        </b-table>
        <nav>
          <b-pagination size="sm" :total-rows="getRowCount(items)" :per-page="perPage" v-model="currentPage" prev-text="Prev" next-text="Next" hide-goto-end-buttons/>
        </nav>
      </b-card>
      </transition>
    </b-col>
  </b-row>
</template>

<script>
import { Switch as cSwitch } from '@coreui/vue'
import usersData from './UsersData'

export default {
  name: 'Feedbacks',
  components: {
    cSwitch 
  },
  props: {
    caption: {
      type: String,
      default: 'Feedbacks'
    },
    hover: {
      type: Boolean,
      default: true
    },
    striped: {
      type: Boolean,
      default: true
    },
    bordered: {
      type: Boolean,
      default: false
    },
    small: {
      type: Boolean,
      default: false
    },
    fixed: {
      type: Boolean,
      default: false
    }
  },
  data: () => {
    return {
      items: usersData.filter((user) => user.id < 42),
      fields: [
        {key: 'id'},
        {key: 'user'},
        {key: 'recieved_on'},
        {key: 'message'},
        {key: 'status'}
      ],
      currentPage: 1,
      perPage: 5,
      totalRows: 0
    }
  },
  computed: {
  },
  methods: {
    getBadge (status) {
      return status === 'Active' ? 'success'
        : status === 'Inactive' ? 'secondary'
          : status === 'Pending' ? 'warning'
            : status === 'Banned' ? 'danger' : 'primary'
    },
    getRowCount (items) {
      return items.length
    },
    feedbackLink (id) {
      return `feedbacks/${id.toString()}`
    },
    rowClicked (item) {
      const userLink = this.feedbackLink(item.id)
      this.$router.push({path: userLink})
    }

  }
}
</script>

<style scoped>
.card-body >>> table > tbody > tr > td {
  cursor: pointer;
}
</style>
