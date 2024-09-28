<template>
    <div id="login">
        <h1>Login</h1>
        <div class="form-inputs">
            <label for="email">Email</label>
            <input type="text" id="email" name="email" v-model="input.email" placeholder="Username" />
        </div>
        <div class="form-inputs">
            <label for="password">Password</label>
            <input type="password" id="password" name="password" v-model="input.password" placeholder="Password" />
        </div>
        <button type="button" v-on:click="login()">Login</button>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: 'Login',
        data() {
            return {
                input: {
                    email: "test@test.com",
                    password: "Password12#"
                }
            }
        },
        methods: {
            async login() {
                if(this.input.email != "" && this.input.password != "") {
                    await axios.post('http://localhost:3313/api/auth/login', this.input).then(response =>{
                        localStorage.setItem('authData', JSON.stringify(response.data.data));
                        this.$emit("authenticated", true);
                        this.$router.replace({ name: "Dashboard" });
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

<style>

#login .form-inputs {
    padding-bottom: 10px;
}

#login .form-inputs label {
    padding-right: 10px;
}

</style>