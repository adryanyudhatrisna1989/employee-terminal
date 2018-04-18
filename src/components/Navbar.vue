<template>
  <nav>
    <div class="nav-wrapper light-blue">
      <div class="container">
        <router-link to="/" class="brand-logo">Terminal A</router-link>
        <ul class="right">
          <li v-if="isLoggedIn"><span class="email white-text">{{currentUser}}</span></li>
          <li v-if="isLoggedIn"><router-link to="/">Home</router-link></li>
          <li v-if="!isLoggedIn"><router-link to="/login">Sign in</router-link></li>
          <li v-if="!isLoggedIn"><router-link to="/register">Sign up</router-link></li>
          <li v-if="isLoggedIn"><button v-on:click="logout" class="btn">Sign out</button></li>
       </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import firebase from 'firebase';
export default {
  name: 'navbar',
  data() {
    return {
      isLoggedIn: false,
      currentUser: false
    };
  },
  created() {
    if (firebase.auth().currentUser) {
      this.isLoggedIn = true;
      this.currentUser = firebase.auth().currentUser.email;
    }
  },
  methods: {
    logout: function() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.go({ path: this.$router.path });
        });
    }
  }
};
</script>

<style scoped>
.email {
  padding-right: 10px;
}
</style>