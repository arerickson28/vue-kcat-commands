<script>
export default {
//   data() {
//     return {
//       message: 'kcat -P -X security.protocol=ssl -X ssl.certificate.location=<changeMe> -X ssl.key.location=<changeMe> -X ssl.ca.location=<changeMe> -b awsomeStageServer:9092 -t my-awesome-topic'
//     }
//   }
props: ['topicData'],
  created() {
    console.log(this.topicData)
  }
}

</script>

<template>
    <h3 class="purple">{{ topicData.topic.topicName }}</h3>
</template>


<style>
.purple {
    border: solid 4px rgb(128, 17, 128);
    padding: 5px;
    margin: 10px;
    &:hover {
        box-shadow: inset 0px 0px 8px 
        purple, 0 0 15px purple ;
    }
}
</style>