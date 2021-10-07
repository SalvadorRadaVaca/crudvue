<template>
    <div class="container">
        <router-link to="/create" class="btn btn-success">Add new employee</router-link>
        <br/><br/>
        <div class="card">
            <div class="card-header">
                Employees
            </div>
            <div class="card-body">
                <table class="table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Email</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="employee in employees" :key="employee.id">
                            <td>{{employee.id}}</td>
                            <td>{{employee.name}}</td>
                            <td>{{employee.email}}</td>
                            <td>
                                <div class="btn-group" role="group" aria-label="">
                                    <router-link :to="{name:'Edit', params:{id:employee.id}}" class="btn btn-info">Edit</router-link>
                                    <button type="button" v-on:click="deleteEmployee(employee.id)" class="btn btn-danger">Delete</button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            employees:[]
        }
    },
    created:function(){
        this.consultEmployees();
    },
    methods:{
        // 
        consultEmployees() {
            fetch('http://localhost/employees/')
            .then(response=>response.json())
            .then((responseData)=>{
                console.log(responseData)
                this.employees = []
                if(typeof responseData[0].success === 'undefined')
                {
                    this.employees=responseData;
                }
            })
            .catch(console.log)
        },

        deleteEmployee(id) {
            console.log(id);
            fetch('http://localhost/employees/?delete='+id)
            .then(response=>response.json())
            .then((responseData)=>{
                console.log(responseData)
                window.location.href="list"
            })
            .catch(console.log)
        }
    }
}
</script>