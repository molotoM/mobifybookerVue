<template>
<div >
    <h1>Welcome Back {{this.$route.params.id.name}}</h1>
    <button type="button"  @click="updateInfo(this.$route.parama.id.user_id)">UPDATE INFORMATION</button>
    <br><br>
    <table id="firstTable">
        <thead>
            <tr>
                <th>vehicle_registration</th>
                <th>vehicle</th>
                <th>book Inspection</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(car, i) in cars" :key="i">
                <td>{{car.vehicle_registration}}</td>
                <td>{{car.make}}<br>{{car.model}}<br>{{car.colour}}</td>
                <td><button type="button" @click="bookInsepection(car.vehicle_id)">BOOK</button></td>
            </tr>
         </tbody>
    </table><br>
    
    <h4>Appointments</h4>
    <table>
        <thead>
            <tr>
                <th>vehicle_registration</th>
                <th>appointmentType</th>
                <th>status</th>
                <th>date</th>
                <th>technician</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(car, i) in appointments" :key="i">
                <td>{{car.car_reg}}</td>
                <td>{{car.app_type}}</td>
                <td>{{car.app_status}}</td>
                <td>{{car.app_date}} </td>  
                <td>{{car.technician}}</td>
            </tr>
         </tbody>
    </table><br><br>
    <button type="button" @click="bookInstallation()">BOOK INSTALLATION</button><br><br>
</div>
</template>

<script>

export default {
    name:'clientRegistration',
    data(){
        return {
            cars:null,
            appointments:null
        }
    },
    methods:{
        getCars(){
        fetch(`https://obscure-everglades-78775.herokuapp.com/getUserId/${this.$route.params.id.user_id}`)
        .then(response=>response.json())
        .then(results=>(this.cars = results.user.data))
        },
        getAppoinments(){
            fetch(`https://obscure-everglades-78775.herokuapp.com/getClientAppointments/${this.$route.params.id.user_id}`)
            .then(results=>results.json())
            .then((response)=>(this.appointments = response.user.data))
        },
        bookInstallation(){
            this.$router.push({name:'newInstall', params: {id:this.$route.params.id.user_id}})

        },
        bookInsepection(carId){
            let reqBody = {
                car:carId,
                client:this.$route.params.id.user_id
            }
            fetch('https://obscure-everglades-78775.herokuapp.com/bookInspection/',{
              method: "POST",
            headers: {
                Accept: "application/json",
                    "Content-Type": "application/json"
             },
            body: JSON.stringify(reqBody)
            })
            .then(results=>(results.json,console.log(results)))
            .then(alert('You successfully added an inspection'))
        },
        updateInfo(user){
            this.$router.push({name:'updateUser', params: {id:this.$route.params.id.user_id}})
        }
    },
   mounted(){
       this.getCars()
       this.getAppoinments()
    }

}
</script>

<style scoped>
table {
  font-family: 'Open Sans', sans-serif;
  width: 750px;
  border-collapse: collapse;
  border: 3px solid #44475C;
  margin: auto;
  align-items: center;
}

table th {
  text-transform: uppercase;
  text-align: left;
  background: #44475C;
  color: #FFF;
  padding: 8px;
  min-width: 30px;
}

table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7D82A8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #D4D8F9;
}
</style>