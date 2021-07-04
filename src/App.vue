<template>
<v-container fluid fill-height>
  <v-layout align-center justify-center>
    <div v-if="signedIn">
      <amplify-photo-picker :photo-picker-config="photoPickerConfig" />
      <amplify-s3-album path="upload/"></amplify-s3-album>
      <amplify-sign-out />
    </div>
    <div v-else>
      <amplify-sign-out />
    </div>
  </v-layout>
</v-container>
</template>

<script>
import { AmplifyEventBus } from 'aws-amplify-vue'
import Auth from '@aws-amplify/auth'

export default {
  props: [],
  data () {
    return {
      signedIn: false,
      photoPickerConfig: {
        header: 'Upload Profile Pic',
        accept: 'image/*',
        path: 'upload/'
      }
    }
  },
  async beforeCreate () {
    try {
      await Auth.currentAuthenticatedUser()
      this.signedIn = true
    } catch (err) {
      this.signedIn = false
      console.log(err)
    }
    AmplifyEventBus.$on('fileUpload', (img) => {
      console.log('img:' + JSON.stringify(img))
    })
  }
}
</script>
