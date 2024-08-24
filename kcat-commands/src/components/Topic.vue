<script>
import { topicData } from '@/data/TopicData';

export default {
  data() {
    return {
      isDisplayedState: false,
      serverSelectionState: topicData.stageServer,
      sslEnvState: "stage"
    }
  },
props: ['topicData'],
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
    handleSelection(e, env) {
      this.setServerSelectionState(e.target.value)
      this.setSslEnvState(env)
      
    }
  }
}

</script>

<template>
    <div>
        <div class="purple" @click=setIsDisplayedState(true)>
        <h3>{{ topicData.topic.topicName }}</h3>
        <div v-if="isDisplayedState">
          <div>
            <input :checked="sslEnvState == 'prod'" v-bind:value="topicData.topic.prodServer"  v-bind:name=" topicData.topic.topicName +'-prodEnv'"  type="radio" @input="(e) => handleSelection(e, 'prod')"/>
            <label>Prod</label>
            <input  :checked="sslEnvState == 'stage'" v-bind:value="topicData.topic.stageServer" v-bind:name=" topicData.topic.topicName +'-stageEnv'" type="radio" @input="(e) => handleSelection(e, 'stage')"/>
            <label>Stage</label>
          </div>
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
    margin: 10px;
    width: 450px;
    &:hover {
        box-shadow: inset 0px 0px 8px 
        purple, 0 0 15px purple ;
    }

h3 {
    font-size: 1.5em;
}
}
</style>