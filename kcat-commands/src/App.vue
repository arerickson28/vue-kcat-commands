<script setup>
import Topic from './components/Topic.vue'
import { topicData, topicData1 } from "./data/TopicData.js"
import store from "store"
import { provide, reactive } from 'vue'
import { ref } from 'vue'

function sortTopicsByTopicName(objects) {
    const n = objects.length;
    let swapped;

    do {
        swapped = false;
        for (let i = 0; i < n - 1; i++) {
            // Compare adjacent strings
            if (objects[i]["topicName"] > objects[i + 1]["topicName"]) {
                // Swap if out of order
                [objects[i]["topicName"], objects[i + 1]["topicName"]] = [objects[i + 1]["topicName"], objects[i]["topicName"]];
                swapped = true;
            }
        }
    } while (swapped);

    return objects;
}

const alphabatizedTopics = sortTopicsByTopicName(topicData1)

let showUpdateBtnState = ref(true)
let showGetNewSSlState = ref(false)

const sslLoc = ref({
    "stage": {
      "ssl_certificate_location": "<changeMe>",
      "ssl_key_location": "<changeMe>",
      "ssl_ca_location": "<changeMe>"
    },
    "prod": {
      "ssl_certificate_location": "<changeMe>",
      "ssl_key_location": "<changeMe>",
      "ssl_ca_location": "<changeMe>"
    }
  })

  function setLocalStorageIfNull(obj) {
    if (store.get("sslLoc") == null) {
      store.set("sslLoc", obj)
    }
  }

  setLocalStorageIfNull(sslLoc.value)

  let sslLocInfoState = ref(store.get("sslLoc"))
  provide('sslLocInfoState', sslLocInfoState)
  function setSslLocInfoState(v) {
    sslLocInfoState.value = v
  }

  let sslEnvState = ref("stage")
  function setSslEnvState(v) {
    sslEnvState.value = v
  }

  function saveToLocalStorage() {
    store.set("sslLoc", sslLocInfoState.value)
  }

function toggleUpdateSSL() {
  showUpdateBtnState.value = !showUpdateBtnState.value
  showGetNewSSlState.value = !showGetNewSSlState.value
}

function handleInput(e, sslType) {
  if (sslEnvState.value == "stage") {

  let tempLocInfoState = sslLocInfoState.value
  tempLocInfoState.stage[sslType] = e.target.value

  setSslLocInfoState(tempLocInfoState)
  } else {
  let tempLocInfoState = sslLocInfoState.value
  tempLocInfoState.prod[sslType] = e.target.value

  setSslLocInfoState(tempLocInfoState)
  }
}

</script>

<template>
  <div class="main">
    <div>
      <h1>Kcat Command Generator</h1>
      <hr>
      </hr>
    </div>

    <div class="TealBox">
      <h3>Set Your SSL Location</h3>
      <div>
        <input value="prod" name="env" type="radio" @input="(e) => setSslEnvState(e.target.value)"/>
        <label>Prod</label>
        <input value="stage" name="env" type="radio" defaultChecked @input="(e) => setSslEnvState(e.target.value)"/>
        <label>Stage</label>
      </div>
      <div class="PartsDiv">
        <h3>ssl.certificate.location = {{sslLocInfoState[sslEnvState].ssl_certificate_location}}</h3>
        <h3>ssl.key.location = {{sslLocInfoState[sslEnvState].ssl_key_location}}</h3>
        <h3>ssl.ca.location = {{sslLocInfoState[sslEnvState].ssl_ca_location}}</h3>
      </div>

      <hr>
      </hr>
      <div v-if="showUpdateBtnState">
        <button @click.stop="toggleUpdateSSL" style="margin:15px">Update</button>
      </div>
      <div v-if="showGetNewSSlState">
        <div class="PartsDiv">
          <div>
            <label>ssl.certificate.location = </label>
            <input @input="(e) => handleInput(e, 'ssl_certificate_location')" type="text" />
          </div>

          <div>
            <label>ssl.key.location = </label>
            <input @input="(e) => handleInput(e, 'ssl_key_location')" type="text" />
          </div>

          <div>
            <label>ssl.ca.location = </label>
            <input @input="(e) => handleInput(e, 'ssl_ca_location')" type="text" />
          </div>
        </div>
        <button @click.stop="() => {toggleUpdateSSL(); saveToLocalStorage()}" style="margin:15px">Save to local storage</button>
        <br></br>
        <button @click.stop="toggleUpdateSSL" style="margin:15px">Cancel</button>
      </div>

    </div>
    <div class="Topics">
      <hr>
      </hr>
      <Topic v-for="topic in alphabatizedTopics" :topic-data={topic} :ssl-loc-info={sslLocInfoState} :key="topic.topicName"></Topic>
      <hr>
      </hr>
    </div>
  </div>
</template>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}

.Topics {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  align-items: center;
}

.TealBox {
  border: solid 4px rgb(0, 128, 128);
  padding: 5px;
  margin: 10px;
  text-align: center;
}

.PartsDiv {
  display: flex;
  justify-content: space-around;
}
</style>
