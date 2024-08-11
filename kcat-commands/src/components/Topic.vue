<script>
export default {
  data() {
    return {
      isDisplayedState: false
    }
  },
props: ['topicData'],
  created() {},
  methods: {
    setIsDisplayedState(bool) {
        this.isDisplayedState = bool
    }
  }
}

</script>

<template>
    <div>
        <div class="purple" @click=setIsDisplayedState(true)>
        <h3>{{ topicData.topic.topicName }}</h3>
        <div v-if="isDisplayedState">
            <button @click.stop="setIsDisplayedState(false)">Collapse</button>
        </div>
    </div>
    </div>
   
    
</template>


<style>
.purple {
    border: solid 4px rgb(128, 17, 128);
    padding: 5px;
    margin: 10px;
    width: 500px;
    &:hover {
        box-shadow: inset 0px 0px 8px 
        purple, 0 0 15px purple ;
    }

h3 {
    font-size: 1.5em;
}
}
</style>