<template>
  <div class="about">
    <h1>About Page ?</h1>
    <UsersList
      :users="users"
      @delete:user="deleteUser"
      @edit:user="editUser"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import UsersList from '@/components/UsersList.vue'

export default {
  name: 'About',
  components: {
    UsersList,
  },
  props: {
  },
  data() {
    return {
      users: []
    }
  },

  mounted() {
    this.getUsers()
  },

  methods: {
    async getUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.users = data
      } catch (error) {
        console.error(error)
      }
    },

    async addUser(user) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(user),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        })
        const data = await response.json()
        this.users = [...this.users, data]
      } catch (error) {
        console.error(error)
      }
    },

    async editUser(id, updatedUser) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedUser),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        })
        const data = await response.json()
        this.users = this.users.map(user => user.id === id ? data : user)
      } catch (error) {
        console.error(error)
      }
    },

    async deleteUser(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'DELETE'
        })
        this.users = this.users.filter(user => user.id !== id)
      } catch (error) {
        console.error(error)
      }
    },
  },
}
</script>
<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

button:hover,
button:active,
button:focus {
  background: #32a95d;
  border: 1px solid #32a95d;
}

.small-container {
  max-width: 680px;
}
</style>
