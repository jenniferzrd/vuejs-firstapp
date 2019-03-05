<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul>
      <li v-for="user in users" :key="user.id">
        <div v-if="edit === user.id">
          <input type="text" v-model="user.name">
          <button v-on:click="editUser(user)">SAVE</button>
        </div>

        <div v-else>
          <button v-on:click="edit = user.id">EDIT</button>
          {{ user.name }}
          {{ user.address.street }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Axios",
      users: [],
      edit: null
    };
  },
  created() {
    axios
      .get(`http://jsonplaceholder.typicode.com/users`)
      .then(response => {
        this.users = response.data;
        this.users.length = 5;
      })
      .catch(e => {
        console.log(e);
      });
  },
  methods: {
    editUser(user) {
      let uri = `http://jsonplaceholder.typicode.com/users/`;
      axios
        .put(uri + user.id)
        .then(() => {
           console.log(this.edit)
          this.edit = null
        })
        .catch(e => {
          console.log(e);
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
