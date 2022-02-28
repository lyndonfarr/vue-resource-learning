<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="form-group">
      <label>Username</label>
      <input v-model="user.username" type="text" class="form-control">
    </div>
    <div class="form-group">
      <label>Mail</label>
      <input v-model="user.email" type="text" class="form-control">
    </div>
    <button @click="submit" class="btn btn-primary">Submit</button>
    <hr>
    <input type="text" class="form-control" v-model="node">
    <br><br>
    <button class="btn btn-primary" @click="fetchData">Get Data</button>
    <ul class="list-group">
      <li :key="u.id" class="list-group-item" v-for="u in users">{{ u.username }} - {{ u.email }}</li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      user: {
        username: '',
        email: '',
      },
      users: [],
      resource: {},
      node: `data`,
    }
  },

  created() {
    const customActions = {
      saveAlt: {
        method: `POST`,
        url: `alternative.json`,
      },
      getData: {
        method: `GET`,
      }
    }
    this.resource = this.$resource(`{node}.json`, {}, customActions)
  },

  methods: {
    submit() {
      // this.$http
      //   .post(`data.json`, this.user)
      //   .then(response => {
      //     console.log(response)
      //   }, error => {
      //     console.log(error)
      //   });
      // this.resource.save({}, this.user);
      this.resource.saveAlt(this.user);
    },
    fetchData() {
      // this.$http
      //   .get(`data.json`)
      //   .then(response => {
      //     return response.json();
      //   })
      //   .then(data => {
      //     let resultArray = [];
      //     Object.entries(data)
      //       .forEach(entry => {
      //         resultArray.push({
      //           id: entry[0],
      //           ...entry[1],
      //         })
      //       });
      //     this.users = resultArray;
      //   });
      this.resource.getData({node: this.node})
        .then(response => {
          return response.json();
        })
        .then(data => {
          // console.log(data);
          // let resultArray = [];
          // Object.entries(data)
          //   .forEach(entry => {
          //     resultArray.push({
          //       id: entry[0],
          //       ...entry[1],
          //     })
          //   });
          // this.users = resultArray;
          const resultArray = [];
          for (let key in data) {
            resultArray.push(data[key]);
          }
          this.users = resultArray;
        });
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
