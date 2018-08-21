<template>
  <div class="recognition">
    <ul>
      <li v-for="(label, key, index) in rekognitionDetectLabels" :key="index">
        Name: {{ label.Name }}, Confidence: {{ label.Confidence }}
      </li>
    </ul>
    <button @click="detectLabels" type="submit">detectLabels</button>
  </div>
</template>

<script>
// import axios from 'axios'
import AWS from 'aws-sdk'

const BUCKET_NAME = 'vue-sample-apps-upload-photos'

AWS.config.region = 'us-east-1'
AWS.config.apiVersions = {rekognition: '2016-06-27'}
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
  IdentityPoolId: 'us-east-1:9c0ee02a-35e4-450d-a2e6-15ae50b9d8b8'
})
AWS.config.credentials.get(function (err) {
  if (!err) {
    console.log('Cognito Identity ID: ' + AWS.config.credentials.identityId)
  }
})

export default {
  name: 'recognition',
  props: {
  },
  data () {
    return {
      rekognitionDetectLabels: []
    }
  },
  created: function () {
    this.rekognitionDetectLabels = [
      { Name: 'a', Confidence: '10' }
    ]
  },
  methods: {
    detectLabels: function () {
      // rekognition
      var rekognition = function () {
        return new AWS.Rekognition()
      }
      var params = {
        Image: {
          Bytes: this.uploadImage,
          S3Object: {
            Bucket: BUCKET_NAME,
            Name: '1534577324638.png'
          }
        }
      }
      rekognition().detectLabels(params, function (err, data) {
        if (err) {
          console.log(err, err.stack) // an error occurred
        } else {
          console.log(data) // successful response
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
