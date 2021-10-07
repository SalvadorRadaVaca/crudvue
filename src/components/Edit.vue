<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Edit new Employees
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="updateRegister">

                    <div class="form-group">
                      <label for="name">Name:</label>
                      <input type="text"
                        class="form-control" required name="name" id="name" v-model="employee.name" aria-describedby="helpId" placeholder="Name">
                      <small id="helpId" class="form-text text-muted">Write the employee name</small>
                    </div>

                    <div class="form-group">
                      <label for="email">Email:</label>
                      <input type="email"
                        class="form-control" required name="email" id="email" v-model="employee.email" aria-describedby="helpId" placeholder="Email">
                      <small id="helpId" class="form-text text-muted">Write the employee email</small>
                    </div>

                    <div class="btn-group" role="group" aria-label="">
                        <button type="submit" class="btn btn-success">Modify</button>
                        <router-link :to="{name:'List'}" class="btn btn-warning">Cancel</router-link>
                    </div>

                </form>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            employee:{}
        }
    },
    created:function(){
        this.getIdInformation();
    },
    methods:{
        getIdInformation() {
            fetch('http://localhost/employees/?consult='+this.$route.params.id)
            .then(response=>response.json())
            .then((responseData)=>{
                console.log(responseData)
                this.employee = responseData[0];
            })
            .catch(console.log)
        },
        updateRegister(){
            var dataSend={id:this.$route.params.id, name:this.employee.name, email:this.employee.email}
            fetch('http://localhost/employees/?update='+this.$route.params.id, {
                method:"POST",
                body:JSON.stringify(dataSend)
            })
            .then(response=>response.json())
            .then((responseData=>{
                console.log(responseData);
                window.location.href='../list'
            }))
        }
    }
}
</script>