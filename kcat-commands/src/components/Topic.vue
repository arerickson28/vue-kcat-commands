<script>
import { topicData } from '@/data/TopicData';

// import { topicData } from '@/data/TopicData';

export default {
  props: [
  'topicData',
  'sslInfo'
],
  data() {
    return {
      topicName: this.topicData.topic.topicName,
      isDisplayedState: false,
      serverSelectionState: this.topicData.topic.stageServer,
      sslEnvState: "stage",
      producerConsumerState: "C",
      kCatString: "",
      sslString: `kcat -C -b ${this.topicData.topic.stageServer} -t ${this.topicData.topic.topicName}`,
      sslBaseString: "-X ssl.certificate.location=<changeMe> -X ssl.key.location=<changeMe> -X security.protocol=ssl -X ssl.ca.location=<changeMe> ",
    }
  },
  mounted() {
    this.setServerSelectionState(this.topicData.topic.stageServer)
    // this.setProducerConsumerState("C")
    this.setKcatString();
  },
  created() {},
  methods: {
    setIsDisplayedState(bool) {
        this.isDisplayedState = bool
    },
    setServerSelectionState(string) {
      this.serverSelectionState = string
      console.log(this.serverSelectionState)
    },
    setSslEnvState(string) {
      this.sslEnvState = string
      // console.log(this.sslInfo.sslLocInfo.sslLocInfoState.stage.ssl_certificate_location)
      console.log(this.sslEnvState)
    },
    setProducerConsumerState(string) {
      this.producerConsumerState = string
    },
    handleEnvSelection(e, env) {
      this.setServerSelectionState(e.target.value)
      this.setSslEnvState(env)
      console.log(this.sslString)
      this.setKcatString()
      // this.getSslString()
    },
    handlePCSelection(e) {
      console.log(e.target.value)
      this.setProducerConsumerState(e.target.value)
      this.setKcatString()
      // console.log(this.topicData)
      // console.log(sslInfo)
    },
    setKcatString() {
      let topicString = ` kcat -${this.producerConsumerState} -b ${this.serverSelectionState} -t ${this.topicName}`
      this.kCatString = this.sslBaseString + topicString
    },
    // getSslString() {
    //   // return "hey"
    //   let env = this.sslEnvState
    //   console.log(`woah: ${this.sslEnvState}`)
    //   return `kcat -${this.producerConsumerState} -b ${this.serverSelectionState} -t ${this.topicData.topicName}`
    //   // console.log(this.sslLocInfo)
    //   // return this.sslInfo.sslLocInfo.sslLocInfoState["stage"].ssl_certificate_location
    //   // return this.sslEnvState
    // //  return  `-X security.protocol=ssl -X ssl.certificate.location=${this.sslInfo.sslLocInfo.sslLocInfoState[this.sslEnvState].ssl_certificate_location} -X ssl.key.location=${this.sslInfo.sslLocInfo.sslLocInfoState[this.sslEnvState].ssl_key_location} -X ssl.ca.location=${this.sslInfo.sslLocInfo.sslLocInfoState[this.sslEnvState].ssl_ca_location}`
    // }
  }
}

//  const sslString = `-X security.protocol=ssl -X ssl.certificate.location=${sslLocInfo[sslEnvState].ssl_certificate_location} -X ssl.key.location=${sslLocInfo[sslEnvState].ssl_key_location} -X ssl.ca.location=${sslLocInfo[sslEnvState].ssl_ca_location}`
//     const kafkaCmd = `kcat -${producerOrConsumer} ${sslString} -b ${serverSelectionState} -t ${topicData.topicName}`

</script>

<template>
    <div>
        <div class="purple" @click=setIsDisplayedState(true)>
         
        <h3>{{ this.topicName }}</h3>
        <div v-if="isDisplayedState">
          <div>
            <!-- <input :checked="sslEnvState == 'prod'" v-bind:value="topicData.topic.prodServer"  v-bind:name=" topicData.topic.topicName +'-prodEnv'"  type="radio" @input="(e) => handleEnvSelection(e, 'prod')"/>
            <label>Prod</label>
            <input  :checked="sslEnvState == 'stage'" v-bind:value="topicData.topic.stageServer" v-bind:name=" topicData.topic.topicName +'-stageEnv'" type="radio" @input="(e) => handleEnvSelection(e, 'stage')"/>
            <label>Stage</label> -->
            <input :checked="sslEnvState == 'prod'" v-bind:value="topicData.topic.prodServer"  v-bind:name=" topicData.topic.topicName +'-prodEnv'"  type="radio" @input="(e) => handleEnvSelection(e, 'prod')"/>
            <label>Prod</label>
            <input :checked="sslEnvState == 'stage'" v-bind:value="topicData.topic.stageServer" v-bind:name=" topicData.topic.topicName +'-stageEnv'" type="radio" @input="(e) => handleEnvSelection(e, 'stage')"/>
            <label>Stage</label>
          </div>
          <hr/>
          <div>
            <input :checked="producerConsumerState == 'P'" value="P"  v-bind:name=" topicData.topic.topicName +'-P'"  type="radio" @input="(e) => handlePCSelection(e)"/>
            <label>Producer</label>
            <input :checked="producerConsumerState == 'C'" value="C" v-bind:name=" topicData.topic.topicName +'-C'" type="radio" @input="(e) => handlePCSelection(e)"/>
            <label>Consumer</label>
          </div>
          <hr/>
          <h3>{{ this.kCatString }}</h3>
        <br/>
            <button @click.stop="setIsDisplayedState(false)">Collapse</button>
        </div>
    </div>
    </div>
   
    
</template>


<style>
.purple {
    border: solid 4px rgb(128, 17, 128);
    padding: 5px 35px 5px 35px;
    margin: 50px;
    /* width: 450px; */
    width: 950px;
    &:hover {
        box-shadow: inset 0px 0px 8px 
        purple, 0 0 15px purple ;
    }
}

h3 {
    font-size: 1.5em;
}

input {
  margin: 15px;
}
</style>