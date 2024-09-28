<template>
    <div id="register" class="container">
        <h1>Register</h1>
        <div class="form-inputs">
            <label for="first_name">First name</label>
            <input type="text" id="first_name" name="first_name" v-model="input.first_name" placeholder="First name" />
        </div>
        <div class="form-inputs">
            <label for="last_name">Last name</label>
            <input type="text" id="last_name" name="last_name" v-model="input.last_name" placeholder="Last name" />
        </div>
        <div class="form-inputs">
            <label for="email">Email</label>
            <input type="text" id="email" name="email" v-model="input.email" placeholder="email" />
        </div>
        <div class="form-inputs">
            <label for="password">Password</label>
            <input type="password" id="password" name="password" v-model="input.password" placeholder="Password" />
        </div>
        <button type="button" v-on:click="register()">Register</button>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: 'Register',
        data() {
            return {
                input: {
                    first_name: "James",
                    last_name: "Camerom",
                    email: "james@cameron.com",
                    password: "Password12#"
                }
            }
        },
        methods: {
            async register() {
                if(this.input.email != "" && this.input.password != "") {
                    await axios.post('http://localhost:3313/api/auth/register', this.input).then(response =>{
                        alert(response.data.message);
                        this.$router.replace({ name: "Login" });
                    }).catch(error => {
                        alert(error.response.data.message);
                    });
                } else {
                    alert("A username and password must be present");
                }
            }
        }
    }
</script>