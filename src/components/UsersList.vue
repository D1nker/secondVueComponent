<template>
  <div class="hello">
    <section></section>
    <h1>Lets fetch some users with nested components</h1>
    <p
      v-if="users.length < 1"
      class="empty-table"
    >
      No Users
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <!-- Shorthand for v-bind -->
        <tr
          :key="user.id"
          v-for="user in users"
        >
          <td v-if="editing === user.id">
            <input
              type="text"
              v-model="user.name"
            >
          </td>
          <td v-else>{{user.name}}</td>
          <td v-if="editing === user.id">
            <input
              type="text"
              v-model="user.email"
            >
          </td>
          <td v-else>{{user.email}}</td>
          <td v-if="editing === user.id">
            <button @click="editUser(user)">Save</button>
            <button
              class="muted-button"
              @click="cancelEdit(user)"
            >Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(user)">Edit</button>
            <!-- Shorthand for v-on -->
            <button @click="$emit('delete:user', user.id)">Delete</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'usersList',
  props: {
    msg: String,
    users: Array,
  },
  data() {
    return {
      editing: null,
    }
  },
  methods: {
    editMode(user) {
      this.cachedUser = Object.assign({}, user)
      this.editing = user.id
    },

    cancelEdit(user) {
      Object.assign(user, this.cachedUser)
      this.editing = null;
    },

    editUser(user) {
      if (user.name === '' || user.email === '') return
      this.$emit('edit:user', user.id, user)
      this.editing = null
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
button {
  margin: 0 0.5rem 0 0;
}

input {
  margin: 0;
}

.empty-table {
  text-align: center;
}
</style>
