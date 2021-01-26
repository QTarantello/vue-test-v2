<template>
  <main>
    <Modal :onAddUser="addUser" :users="state.users" />
    <Table
      @onSortUsers="sortUsers"
      :sortBy="state.sortBy"
      :users="state.users"
    />
  </main>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'
import Modal from './components/Modal.vue'
import Table from './components/Table.vue'
import sortUsersUtil from './components/utilities/useSorting.js'

const makeUser = ({ name, phone }) => ({
  uuid: uuidv4(),
  name,
  phone,
  children: []
})

const initialState = () => {
  const json = localStorage.getItem('state')

  let initial
  if (json !== null) {
    initial = JSON.parse(json)
  } else {
    initial = {
      sortBy: {
        key: 'name',
        direction: 'asc'
      },
      users: [makeUser({ name: 'Вася', phone: '8-800-555-35-35' })]
    }
  }

  return initial
}

const reduceUsers = (users, { name, phone, parentUuid }) => {
  const newUser = makeUser({ name, phone })

  if (parentUuid === null) {
    return [...users, newUser]
  } else {
    return users.map(user => {
      if (user.uuid === parentUuid) {
        return { ...user, children: [...user.children, newUser] }
      } else {
        return user
      }
    })
  }
}

const reduceSortBy = (prevSortBy, key) => ({
  key,
  direction:
    prevSortBy.key === key
      ? prevSortBy.direction === 'desc'
        ? 'asc'
        : 'desc'
      : prevSortBy.direction
})

export default {
  name: 'App',
  components: {
    Modal,
    Table
  },

  data () {
    return {
      state: initialState()
    }
  },

  methods: {
    addUser (payload) {
      this.state.users = reduceUsers(this.state.users, payload)
      localStorage.setItem('state', JSON.stringify(this.state))
    },

    sortUsers (key) {
      const sortBy = reduceSortBy(this.state.sortBy, key)

      this.state.users = sortUsersUtil(this.state.users, sortBy)
      this.state.sortBy = sortBy

      localStorage.setItem('state', JSON.stringify(this.state))
    }
  }
}
</script>

<style scoped>
* {
  font-family: Helvetica, Arial, sans-serif;
  margin: 0;
  padding: 0;
}

</style>
