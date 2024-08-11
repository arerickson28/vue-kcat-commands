<script setup>
import Project from './components/Project.vue'
import SslLoc from './components/SslLoc.vue'
import { topicData } from "./data/TopicData.js"
import store from "store"
import { reactive } from 'vue'
import { ref } from 'vue'

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
  console.log("heyy", sslType)
}

</script>

<template>
  <div class="main">
    <div>
      <h1>Kcat Command Generator</h1>
      <hr>
      </hr>
    </div>

    <!-- <SslLoc></SslLoc> -->
    <div class="TealBox">
      <h3>Your SSL Location</h3>
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

        <button @click.stop="toggleUpdateSSL" style="margin:15px">Save to local storage</button>
        <!-- onClick = {() => {
                        toggleUpdateSSL()
                        saveToLocalStorage()
                        } } -->
        <br></br>
        <button @click.stop="toggleUpdateSSL" style="margin:15px">Cancel</button>
        <!-- onClick = {() => {toggleUpdateSSL()} } -->

      </div>

    </div>

    <div>
      <hr>
      </hr>
      <project v-for="project in topicData" :project-data={project} :key="project.name"></project>
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
