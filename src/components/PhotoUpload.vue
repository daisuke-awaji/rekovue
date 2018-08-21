<template>
  <div class="photo-upload">
    <div v-if="!viewImage">
      <h2>Select an image.</h2>
      <input @change="selectedImage" type="file" name="file">
    </div>
    <div v-else>
      <img class="view-image" v-show="viewImage" :src="viewImage"/>
      <br />
      <button @click="removeImage">Remove image</button>
    </div>
    <br />
    <button @click="upload" type="submit">Upload Photo</button>
  </div>
</template>

<script>
// import axios from 'axios'
import AWS from 'aws-sdk'
const BUCKET_NAME = 'vue-sample-apps-upload-photos'

AWS.config.region = 'us-east-1'
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
  IdentityPoolId: 'us-east-1:9c0ee02a-35e4-450d-a2e6-15ae50b9d8b8'
})
AWS.config.credentials.get(function (err) {
  if (!err) {
    console.log('Cognito Identity ID: ' + AWS.config.credentials.identityId)
  }
})

export default {
  name: 'PhotoUpload',
  props: {
  },
  data () {
    return {
      viewImage: null,
      uploadImage: null
    }
  },
  created: function () {
  },
  methods: {
    selectedImage: function (e) {
      e.preventDefault()
      let files = e.target.files
      let reader = new FileReader()
      reader.onload = (e) => {
        this.viewImage = e.target.result
      }
      reader.readAsDataURL(files[0])
      this.uploadImage = files[0]
    },
    removeImage: function (e) {
      this.viewImage = ''
    },
    upload: function () {
      var s3 = function () {
        return new AWS.S3({params: {Bucket: BUCKET_NAME}})
      }
      let file = this.uploadImage
      let timestamp = new Date().getTime()
      let filename = timestamp + '.png'
      var params = {
        Key: filename,
        Body: file,
        ACL: 'public-read'
      }
      s3().putObject(params, function (err, data) {
        if (err) {
          console.log(err, err.stack) // an error occurred
          alert('upload failed')
        } else {
          console.log(data) // successful response
          alert('upload success')
        }
      })
    }
  }
}
</script>

<style scoped>
  .view-image {
    width: 30%
  }
</style>
