<script setup>
import Project from './components/Project.vue'
import Topic from './components/Topic.vue'
import { topicData } from "./data/TopicData.js"

</script>

<template>
  <div class="main">
    <div>
      <h1>Kcat Command Generator</h1>
      <hr></hr>
    </div>
    
    <div class="TealBox">
      <h3>Your SSL Location</h3>
      <div>
        <input value="prod" name="env" type="radio" />
        <label>Prod</label>
        <input value="stage" name="env" type="radio" defaultChecked />
        <label>Stage</label>
      </div>
      <div class="PartsDiv">
        <h3>ssl.certificate.location = hey</h3>
        <h3>ssl.key.location = hey</h3>
        <h3>ssl.ca.location = hey</h3>
      </div>

      <hr></hr>
      <button style="margin:15px">Update</button>
    </div>

    <div>
       <hr></hr>
       <project v-for="project in topicData" :project-data = {project} :key="project.name"></project>
       
       <hr></hr>
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
