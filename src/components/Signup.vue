<template>
    <div class="container">
        <div class="row py-4 d-flex flex-column align-items-center justify-content-center middle-content">
            <div class="col-md-8">
                <h3 class="text-muted text-start text-monospace">Create your account</h3>
                <hr>
                <form @submit.prevent="submitForm">
                    <div class="form-group my-2 text-start">
                        <input class="form-control" type="username" placeholder="Enter you username" name="username" v-model="username">
                        <small v-if="errors.username" class="text-danger">{{ errors.username }}</small>
                    </div>
                    <div class="form-group my-2 text-start">
                        <input class="form-control" type="password" placeholder="Enter your password" name="password" v-model="password">
                        <small v-if="errors.password" class="text-danger">{{ errors.password }}</small>
                    </div>
                    <div class="form-group my-2 text-start">
                        <input class="form-control" type="password" placeholder="Enter your password" name="password2" v-model="password2">
                        <small v-if="errors.password2" class="text-danger">{{ errors.password2 }}</small>
                    </div>
                    <div class="form-group my-2 d-grid gap-2">
                        <button type="submit" class="btn btn-primary">Sigun to the system</button>
                    </div>
                    <div class="form-group my-2 text-start d-grid gap-2">
                        <p>
                            Already have an account fot the system
                            <router-link class="text-decoration-none" to="/">Click</router-link> here to login
                        </p>
                    </div>
                </form>
                <hr>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Signup',
    data() {
        return {
            username: "",
            password: "",
            password2: "",
            errors: {
                username: "",
                password: "",
                password2: "",
            }
        }
    },
    methods: {
        isValidForm() {
            let valid = true;
            if(!this.username) {
                this.errors.username = "The field cannot be blank";
            }
            else {
                this.errors.username = ""
            }
            if(!this.password) {
                this.errors.password = "The field cannot be blank";
            }
            else {
                this.errors.password = ""
            }
            if(this.password && this.password2 && this.password !== this.password2) {
                this.errors.password2 = "Passwords missmatched";
            }
            else {
                this.errors.password2 = "";
            }
            if(this.errors.username || this.errors.password || this.errors.password2) {
                valid = false;
            }
            return valid;
        },  
        submitForm() {
            if(this.isValidForm()) {
                const url = "/auth/users/";
                axios.post(url, {username: this.username, password: this.password})
                .then(response => {
                    this.$router.push('/')
                    this.username = "";
                    this.password = "";
                    this.password2 = ""
                })
                .catch(error => {
                    console.log(error.response.data)
                    if(error.response.data.username) {
                        this.errors.username = error.response.data.username.join('')
                    }
                    else {
                        this.errors.username = ""
                    }
                })
            }
        }
    }
}

</script>

<style scoped lang="scss">

    .middle-content {
        height: 100vh;
    }

</style>