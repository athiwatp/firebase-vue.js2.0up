<template>
  <div class="hello">
    <ul>
      <li class="user" v-for="user in users" transition>
        <span>{{user.name}} - {{user.email}}</span>
        <button v-on:click="removeUser(user)">X</button>
      </li>
    </ul>
    <form id="form" v-on:submit.prevent="addUser">
      <input v-model="newUser.name">
      <input v-model="newUser.email">
      <input type="submit" value="Add User">
    </form>
    <ul class="errors">
      <li v-show="!validation.name">Name cannot be empty.</li>
      <li v-show="!validation.email">Please provide a valid email address.</li>
    </ul>
  </div>
</template>

<script>
var firebase = require('firebase')
var config = {
  apiKey: 'AIzaSyBUNZ9z0AF3_6NvJAjmWeaAu1L_WlQjuA0',
  authDomain: 'vue1234-f8cf6.firebaseapp.com',
  databaseURL: 'https://vue1234-f8cf6.firebaseio.com',
  storageBucket: 'vue1234-f8cf6.appspot.com',
  messagingSenderId: '415861277156'
}
firebase.initializeApp(config)
var Users = firebase.database().ref('users')
var emailRE = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
export default {
  name: 'inputView',
  data () {
    return {
      users: [],
      newUser: {
        name: '',
        email: ''
      }
    }
  },
  mounted () {
    let vm = this
    Users.on('child_added', function (snapshot) {
      var item = snapshot.val()
      item.id = snapshot.key
      vm.users.push(item)
      console.log(vm.users)
    })
    Users.on('child_removed', function (snapshot) {
      var id = snapshot.key
      vm.users.$remove(vm.users.find(user => user.id === id))
    })
    Users.on('child_changed', function (snapshot) {
      var id = snapshot.key
      console.log(id)
    })
  },
  computed: {
    validation: function () {
      return {
        name: !!this.newUser.name.trim(),
        email: emailRE.test(this.newUser.email)
      }
    },
    isValid: function () {
      var validation = this.validation
      return Object.keys(validation).every(function (key) {
        return validation[key]
      })
    }
  },
  methods: {
    addUser: function () {
      if (this.isValid) {
        Users.push(this.newUser)
        this.newUser.name = ''
        this.newUser.email = ''
      }
    },
    removeUser: function (user) {
      firebase.database().ref('users/' + user.id).remove()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
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
