<script>
import Topic from './Topic.vue'
export default {
  props: ['projectData'],
  created() {
    console.log(this.projectData.project.name)
  },
  components: {
    Topic
  }
}

</script>

<template>
    <h1>{{projectData.project.name}}</h1>
    <hr></hr>
    <div class="BodyDiv">
        <Topic v-for="topic in projectData.project.topics" :topic-data = {topic} :key="topic.topicName"></Topic>
    </div>

   

</template>

<style>

.BodyDiv {
    display: flex;
    flex-direction: row;
    justify-content: space-around;

}

</style>
