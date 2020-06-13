<template>
  <div id="top">
    <Home v-if="!isLogin"></Home>
    <Editor v-if="isLogin" :user="userData"></Editor>
    <router-link :to="{ name: 'terms' }">利用規約</router-link>

    <!-- <vue-loading :to="{name: 'HelloWorld' }" type="spin" color="#333" :size="{ width: '50px', height: '50px' }"></vue-loading> -->
  </div>
</template>

<script>
import Home from "../components/Home"
import Editor from "../components/Editor"
import ViewVue from './View.vue';

export default {
  name: "top",
  data() {
    return {
      isLogin: false,
      userData: null,
      // loading: true
    };
  },
  created: function() {
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
      if (user) {
        this.isLogin = true;
        this.userData = user;
        // this.loading = false;
      } else {
        this.isLogin = false;
        this.userData = null;
      }
    });
  },
  components: {
    Home: Home,
    Editor: Editor,
    VueLoading: ViewVue
  }
};
</script>
<style lang="scss">
//   .loader,
// .loader:after {
//   border-radius: 50%;
//   width: 10em;
//   height: 10em;
// }
// .loader {
//   margin: 60px auto;
//   font-size: 10px;
//   position: relative;
//   text-indent: -9999em;
//   border-top: 1.1em solid rgba(55,210,199, 0.2);
//   border-right: 1.1em solid rgba(55,210,199, 0.2);
//   border-bottom: 1.1em solid rgba(55,210,199, 0.2);
//   border-left: 1.1em solid #37d2c7;
//   -webkit-transform: translateZ(0);
//   -ms-transform: translateZ(0);
//   transform: translateZ(0);
//   -webkit-animation: load8 1.1s infinite linear;
//   animation: load8 1.1s infinite linear;
// }
// @-webkit-keyframes load8 {
//   0% {
//     -webkit-transform: rotate(0deg);
//     transform: rotate(0deg);
//   }
//   100% {
//     -webkit-transform: rotate(360deg);
//     transform: rotate(360deg);
//   }
// }
// @keyframes load8 {
//   0% {
//     -webkit-transform: rotate(0deg);
//     transform: rotate(0deg);
//   }
//   100% {
//     -webkit-transform: rotate(360deg);
//     transform: rotate(360deg);
//   }
// }

</style>
