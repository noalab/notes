<template>
  <div class="signin">
    <div v-if="!signedIn">
      <amplify-authenticator></amplify-authenticator>
    </div>
    <div v-if="signedIn">
      <amplify-sign-out></amplify-sign-out>
    </div>
  </div>
</template>
<script>
import { AmplifyEventBus } from 'aws-amplify-vue'
import { Auth } from 'aws-amplify'
export default {
  name: 'SignIn',
  data () {
    return {
      signedIn: false
    }
  },
  async beforeCreate() {
    try {
      await Auth.currentAuthenticatedUser()
      this.signedIn = true
    } catch (err) {
      this.signedIn = false
    }
    AmplifyEventBus.$on('authState', info => {
      if (info === 'signedIn') {
        this.signedIn = true
      } else {
        this.signedIn = false
      }
    })
  }
}
</script>
<style>
.signin {
  margin: 20px;
}
@media screen and (max-width: 480px) {
  .signin div {
    min-width: 0;
  }
}
</style>