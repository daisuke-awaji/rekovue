<template>
  <div class="photo-list">
    <h1>{{ url }}</h1>
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

const s3 = new AWS.S3({params: {Bucket: BUCKET_NAME}})

export default {
  name: 'PhotoList',
  props: {
  },
  data () {
    return {
      photoUrls: [],
      url: null
    }
  },
  created: function () {
    var params = {
      Bucket: BUCKET_NAME
    }
    s3.listObjects(params, function (err, data) {
      if (err) {
        console.log(err, err.stack) // an error occurred
      } else {
        console.log(data) // successful response
        this.url = 'https://s3.amazonaws.com/vue-sample-apps-upload-photos/' + data.Contents[0].Key
      }
    })
  },
  methods: {
  }
}
</script>

<style scoped>
  .view-image {
    width: 20%
  }
</style>
