<template>
    <form>
    <h5>Please fill in the following to book an inspection</h5>
    <br /><label>Vehicle reg no</label>
    <input type="text" placeholder="Enter number plate" v-model="vehicle.regNO" />
    <br /><br /><label>Model</label>
    <input type="text" placeholder="Car Model" v-model="vehicle.model" />
    <br /><br /><label>Make</label>
    <input type="text" placeholder="car make" v-model="vehicle.make"/>
    <br /><br /><label>Colour</label>
    <input type="text" placeholder="colour of the car" v-model="vehicle.colour"/>
     <br /><br /><button type="button" @click="goBack()">back</button>
    <button type="button" @click="add()">Submit</button>
  </form>
</template>

<script>
export default {
  name: "newInstall",
  data() {
    return {
      vehicle: {
        clientNo:this.$route.params.id,
        regNO: "",
        model: "",
        make: "",
        colour: ""
      }
    };
  },
  methods: {
    add() {
      debugger;
      var newObj = {
        clientNum:this.clientNo,
        reg: this.vehicle.regNO,
        model: this.vehicle.model,
        make: this.vehicle.make,
        color: this.vehicle.colour
      };
      fetch("https://obscure-everglades-78775.herokuapp.com/addNewAppointment", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify(newObj)
      })
      .then(response=>{response.json(),alert('SUBMMITTED')})
      .then(res=>this.$router.go(-1))
      .catch(error => {
          console.log("This is the error: ", error);
      });
        
    },
    goBack(){ 
    }
  }
}
</script>
