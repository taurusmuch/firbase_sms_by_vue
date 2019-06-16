<template>
  <div id="container">
    <input type="text" id="phone_no" placeholder="phone" value="+886932234045">
    <button type="submit" id="sign_btn" @click="onSignInSubmit">LOGIN</button>
    <br>
    <br>
    <input type="text" id="code" placeholder="" value="123456">
    <button type="submit" id="check_code" @click="checkCode">check code</button>
  </div>
</template>

<script>
/* eslint-disable */
import * as firebase from 'firebase/app';
import 'firebase/auth';

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      firebaseConfig: {
        apiKey: "AIzaSyDi74FvJaQxHU8olCriCWu4K_gG5XzNEs0",
        authDomain: "trysms-99648.firebaseapp.com",
        databaseURL: "https://trysms-99648.firebaseio.com",
        projectId: "trysms-99648",
        storageBucket: "trysms-99648.appspot.com",
        messagingSenderId: "123070879436",
        appId: "1:123070879436:web:b170bbcbe4898bef"
      }
    }
  },
  mounted(){
    console.log(firebase);
    firebase.initializeApp(this.firebaseConfig);
    firebase.auth().useDeviceLanguage();
    window.recaptchaVerifier = new firebase.auth.RecaptchaVerifier('sign_btn', {
          'size': 'invisible',
          'callback': function(response) {
            // reCAPTCHA solved, allow signInWithPhoneNumber.
            //onSignInSubmit();
          }
      });
  },
  methods: {
    getPhoneNumberFromUserInput(){
        console.log(document.getElementById('phone_no').value);
        return document.getElementById('phone_no').value;
    },
    onSignInSubmit(){
      var appVerifier = window.recaptchaVerifier;
      var phoneNumber = this.getPhoneNumberFromUserInput();
      firebase.auth().signInWithPhoneNumber(phoneNumber, appVerifier)
          .then(function (confirmationResult) {
              console.log('SMS sent');
            // SMS sent. Prompt user to type the code from the message, then sign the
            // user in with confirmationResult.confirm(code).
            window.confirmationResult = confirmationResult;
          }).catch(function (error) {
            console.log(error);
            console.log('SMS not sent')
            // Error; SMS not sent
            // ...
          });
    },
    getCodeFromUserInput(){
      return document.getElementById('code').value;
    },
    checkCode(){
      var code = this.getCodeFromUserInput();
      //console.log(code);
      confirmationResult.confirm(code).then(function (result) {
        console.log('successfully login')
        // User signed in successfully.
        var user = result.user;
        // ...
      }).catch(function (error) {
        console.log('no login');
        // User couldn't sign in (bad verification code?)
        // ...
      });
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
