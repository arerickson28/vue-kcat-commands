<script setup>
import Topic from './components/Topic.vue'

</script>

<template>
  <div class="main">
  <h1>Kcat Command Generator</h1>
  <hr></hr>
      <div>
        <div class="TealBox">
        <h3>Your SSL Location</h3>
        <!-- <div>
                    <input value = "prod" name="env" type="radio" onInput={(e) => setSslEnvState(e.target.value)}/>
                    <label>Prod</label>
                    <input value = "stage" name="env" type="radio" defaultChecked onInput={(e) => setSslEnvState(e.target.value)} />
                    <label>Stage</label>
        </div> -->
        <div class = "PartsDiv">
        <h3>ssl.certificate.location = hey</h3>
        <h3>ssl.key.location = hey</h3>
        <h3>ssl.ca.location = hey</h3>
        </div>
        <hr></hr>
        </div>
        </div>

  <div>
    <Topic />
  </div>
</div>
</template>

<style scoped>

.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.TealBox {
  border: solid 4px rgb(0,128,128);
  padding: 5px;
  margin: 10px;
  text-align: center;
  /* display: flex; */
  /* flex-direction: column; */
  /* justify-content: center; */
  /* align-items: center; */
}

.PartsDiv {
  display: flex;
  justify-content: space-around;
}

</style>
