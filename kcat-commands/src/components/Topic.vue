<script>
import { inject } from 'vue';
import { topicData } from '@/data/TopicData';


export default {
  props: [
  'topicData',
  'sslInfo'
],
  data() {
    return {
      sslInfo: inject('sslLocInfoState'),
      topicName: this.topicData.topic.topicName,
      isDisplayedState: false,
      serverSelectionState: this.topicData.topic.stageServer,
      sslEnvState: "stage",
      producerConsumerState: "C",
      kCatString: "",
      sslString: `kcat -C -b ${this.topicData.topic.stageServer} -t ${this.topicData.topic.topicName}`,
    }
  },
  mounted() {
    this.setServerSelectionState(this.topicData.topic.stageServer)
    this.setKcatString();
  },
  created() {},
  methods: {
    setIsDisplayedState(bool) {
        this.isDisplayedState = bool
    },
    setServerSelectionState(string) {
      this.serverSelectionState = string
    },
    setSslEnvState(string) {
      this.sslEnvState = string
    },
    setProducerConsumerState(string) {
      this.producerConsumerState = string
    },
    handleEnvSelection(e, env) {
      this.setServerSelectionState(e.target.value)
      this.setSslEnvState(env)
      console.log(this.sslString)
      this.setKcatString()
    },
    handlePCSelection(e) {
      console.log(e.target.value)
      this.setProducerConsumerState(e.target.value)
      this.setKcatString()
    },
    setKcatString() {
      let certificate = this.sslInfo[this.sslEnvState]["ssl_certificate_location"]
      let key = this.sslInfo[this.sslEnvState]["ssl_key_location"]
      let ca = this.sslInfo[this.sslEnvState]["ssl_ca_location"]
      let topicString = ` kcat -${this.producerConsumerState} -b ${this.serverSelectionState} -t ${this.topicName}`
      let sslEnvString = `-X ssl.certificate.location=${certificate} -X ssl.key.location=${key} -X security.protocol=ssl -X ssl.ca.location=${ca} `
      this.kCatString = sslEnvString + topicString
    },
  }
}

</script>

<template>
    <div>
      <div class="purple" @click=setIsDisplayedState(true)>
        <h3>{{ this.topicName }}</h3>
        <div v-if="isDisplayedState">
          <div>
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
          <!-- <p>-X ssl.certificate.location=myStageLoc -X ssl.key.location=myStageKey -X security.protocol=ssl -X ssl.ca.location=mystageSaLoc kcat -C -b terrificStageServer:9092 -t my-terrific-topic</p> -->
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

button {
  margin: 15px;
}
</style>