<template>

    <main>
        <table border="1" class="table table-hover table-dark ">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Name</th>
                    <th scope="col">Email</th>
                    <th scope="col">View more</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(user, index) in users" :key="user.id">
                    <td>{{ index }}</td>
                    <td>{{ user.name }}</td>
                    <td>{{ user.email }}</td>
                    <td>
            <!-- Button trigger modal -->
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="
                loadDetail(
                  street,
                  city,
                  zipcode
                )
              "
            >
              More information
            </button>
            </td>
                </tr>
               
            </tbody>
        </table>
    </main>
</template>

<script>


import axios from 'axios'
export default {
    
    data() {
        
        return {
           users:[]
        };
    },

    mounted (){
this.getUsers();

    },
    methods: {
        getUsers(){
            axios
            .get('https://jsonplaceholder.typicode.com/users')
                     .then( response => {
                        this.users = response.data
                        
                        }).catch(e=> console.log(e))
        },
        
    },
 
};
</script>
