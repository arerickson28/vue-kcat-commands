<script>
export default {
  data() {
    return {
        showUpdateBtnState: true,
        showGetNewSSlState: false
    }
  },
  methods: {
    toggleUpdateSSL() {
      this.showGetNewSSlState = !this.showGetNewSSlState
      this.showUpdateBtnState = !this.showUpdateBtnState
    },
    handleInput(e, sslType) {
        // if (sslEnvState == "stage") {

        // let tempLocInfoState = sslLocInfoState
        // tempLocInfoState.stage[sslType] = e.target.value

        // setSslLocInfoState(tempLocInfoState)
        // } else {
        // let tempLocInfoState = sslLocInfoState
        // tempLocInfoState.prod[sslType] = e.target.value

        // setSslLocInfoState(tempLocInfoState)
        // }
        console.log("heyy", sslType)
    }
  }
}

</script>

<template>
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
      <div v-if="showUpdateBtnState" >
        <button @click.stop="toggleUpdateSSL" style="margin:15px">Update</button>
      </div>
      <div v-if="showGetNewSSlState">
        <div class = "PartsDiv">
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
            <button @click.stop="toggleUpdateSSL" style="margin:15px" >Cancel</button>
            <!-- onClick = {() => {toggleUpdateSSL()} } -->

      </div>
      
    </div>
 
      
</template>


<style>
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