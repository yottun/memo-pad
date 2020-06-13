<template>
  <div id="home">
    <h1>{{ msg }}</h1>
    <button @click="googleLogin">Googleアカウントでログイン</button>
    <div class="test"></div>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      msg: "Welcome to Yottun's MemoPad"
    };
  },
  methods: {
    googleLogin: function() {
      firebase
        .auth()
        .signInWithRedirect(new firebase.auth.GoogleAuthProvider());
    }
  }
};
</script>

<style lang="scss">
    button {
        margin-bottom: 20px;
    }
</style>