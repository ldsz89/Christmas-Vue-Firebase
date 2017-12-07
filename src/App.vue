<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Christmas List Vue-Firebase Application</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Member</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addMember">
          <div class="form-group">
            <label for="memberName">Name:</label>
            <input type="text" id="memberName" class="form-control" v-model="newMember.name" />
          </div>
          <div class="form-group">
            <label for="memberGift">Gift:</label>
            <input type="text" id="memberGift" class="form-control" v-model="newMember.gift.name" />
          </div>
          <input type="submit" class="btn btn-primary" value="Add Member" />
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Members List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Member
              </th>
              <th>
                Gifts
              </th>
              <th>
                Delete
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="member in members">
              <td>
                {{ member.name }}
              </td>
              <td>
                {{ member.gift.name }}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeMember(member)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyCsMmS7TtPL8BHRCQPvfgINmCk3b4KNdrc",
  authDomain: "christmas-a14ca.firebaseapp.com",
  databaseURL: "https://christmas-a14ca.firebaseio.com",
  projectId: "christmas-a14ca",
  storageBucket: "christmas-a14ca.appspot.com",
  messagingSenderId: "212981809423"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let membersRef = db.ref('members');

export default {
  name: 'app',
  firebase: {
    members: membersRef
  },
  data () {
    return {
      newMember: {
        name: '',
        gift: {
          name: ''
        }
      }
    }
  },
  methods: {
    addMember: function() {
      membersRef.push(this.newMember)
      this.newMember.name = '';
      this.newMember.gift.name = '';
    },
    removeMember: function(member) {
      membersRef.child(member['.key']).remove();
      toastr.success("Member removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>
