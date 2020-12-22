<template>
  <form>
    <h5>Enter your new information</h5>
    <br /><br /><label>full Name</label>
    <input type="text" v-model="user.name" />
    <br /><br /><label>Physical Address</label>
    <input type="text" v-model="user.address"/>
    <br /><br /><label>Cellphone number</label>
    <input type="text" v-model="user.number"/>
    <br /><br /><label>Email Address</label>
    <input type="text" v-model="user.email"/>
    <br /><br /><button type="button" @click="addUser()">Update</button>
  </form>
</template>

<script>
export default {
  name: "clientRegistration",
  data() {
    return {
      user: {
        loggedUser:null,
        name: loggedUser.name,
        address: loggedUser.address,
        number: loggedUser.cellphone,
        email:loggedUser.email
        
      }
    };
  },
  methods: {
    addUser() {
      debugger;
      var isTechnician = ""

      if(this.user.userType == "client")
        isTechnician=false;
      else
        isTechnician=true;

      let newUser = {
        _name: this.user.name,
        _address: this.user.address,
        _cellphone_number: this.user.number,
        _email :this.user.email
      };
      fetch("https://obscure-everglades-78775.herokuapp.com/updateUser/", {
        method: "PATCH",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify(newUser)
      })
      .then(alert('You have successfully Updated your personal Information'))
      .then(this.$router.go(-1))
      .catch(error => {
          console.log("This is the error: ", error);
        });
    },
    getCars(){
        fetch(`https://obscure-everglades-78775.herokuapp.com/getUserInfo${this.id}`)
        .then(data=>this.loggedUser=data.json())
    }
  },
   mounted(){
       this.getCars()
   }
};
</script>
