<template>
  <div>
    <h1>{{ msg }}</h1>
    <ul v-if="posts">
      <!-- <li v-for="post in posts" :key="post.id"> -->
      <li v-for="(post, index) in posts" :key="post.id">
        <div v-if="editPosts === post.id">
          <div id="container-li">
            <input v-on:keyup.13="updatePosts(post)" v-model="post.title">
            <button v-on:click="updatePosts(post)">SAVE</button>
          </div>
        </div>

        <div v-else>
          <button v-on:click="editPosts = post.id">EDIT</button>
          <button v-on:click="deletePosts(post.id, index)">DELETE</button>
          {{ post.title }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Test",
  data() {
    return {
      msg: "FirstRoute",
      posts: [],
      postBody: "",
      editPosts: null
    };
  },
  created() {
    fetch(`http://jsonplaceholder.typicode.com/posts`)
      .then(response => response.json())
      .then(data => {
        this.posts = data;
        this.posts.length = 5;
      });
  },
  methods: {
    deletePosts(id, index) {
      fetch(`http://jsonplaceholder.typicode.com/posts` + id, {
        method: "DELETE"
      }).then(() => this.posts.splice(index, 1));
    },
    updatePosts(post) {
      fetch(`http://jsonplaceholder.typicode.com/posts` + post.id, {
        body: JSON.stringify(post),
        method: "PUT",
        headers: {
          // "Content-Type":"x-www-form-urlencoded"
          "Content-Type": "application/json"
        }
      }).then(() => (this.editPosts = null));
    }
  }

  // AXIOS
  //     created() {
  //   axios
  //     .get(`http://jsonplaceholder.typicode.com/posts`)
  //     .then(response => {
  //       // JSON responses are automatically parsed.
  //       this.posts = response.data;
  //       this.posts.length = 5;
  //     })
  //     .catch(e => {
  //       console.log(e);
  //     });
  // }
  // methods: {
  //     deletePosts(id) {
  //       let uri = `http://jsonplaceholder.typicode.com/posts/`;
  //       axios.delete(uri + id)
  //       .then (response => {
  //       this.posts.splice(0, 1);
  //   })
  //   .catch(e => {
  //       console.log(e);
  //     });
  // },
  // editPosts(id) {
  //   let data = {
  //   "title": "test",
  //   "body": "test",
  //   }
  //   let uri = `http://jsonplaceholder.typicode.com/posts/`;
  //   axios.put(uri + id,{data})
  //   .then (response => {
  //     console.log(response);
  //     this.title = response.config.data.title;
  //     this.body = response.config.data.body;
  //           })
  //   .catch(e => {
  //       console.log(e);
  //     });
  // }
  // }
  // END OF AXIOS
};
</script>

<style>
ul {
  text-align: left;
}

li {
  list-style: none;
}

#title {
  font-weight: bold;
  color: darkslategray;
}

#body {
  color: grey;
}

/* #container-li {
  border: 1px solid grey;
  padding: 5px;
  margin-bottom: 5px;
} */
</style>

