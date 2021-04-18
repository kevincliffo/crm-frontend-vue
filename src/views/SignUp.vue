<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Sign Up</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label for="">Email</label>
                        <div class="control">
                            <input v-model="username" type="email" name="email" id="email" class="input">
                        </div>
                    </div>

                    <div class="field">
                        <label for="">Password</label>
                        <div class="control">
                            <input v-model="password" type="password" name="password" id="password" class="input">
                        </div>
                    </div>

                    <div class="field">
                        <label for="">Repeat Password</label>
                        <div class="control">
                            <input v-model="password2" type="password" name="password2" id="password2" class="input">
                        </div>
                    </div>

                    <div v-if="errors.length" class="notification is-danger">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">
                                Submit
                            </button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import {toast} from 'bulma-toast'

    export default {
        name:'SignUp',
        data(){
            return {
                username:'',
                password:'',
                password1:'',
                errors:[]
            }
        },
        methods:{
            submitForm(){
                this.errors = []

                if(this.username === ''){
                    this.errors.push('The username is missing')
                }
                if(this.password === ''){
                    this.errors.push('The password is too short')
                }
                if(this.password2 !== this.password){
                    this.errors.push('The passwords are not matching')
                }
                if(!this.errors.length){
                    const formData = {
                        username: this.username,
                        password:this.password,
                    }
                    axios
                        .post('api/v1/users/', formData)
                        .then(response =>{
                            toast({
                                message:'Account was created please log in',
                                type:'is-success',
                                dismissible:true,
                                pauseOnHover:true,
                                duration:2000,
                                position:'bottom-right',
                            })
                            this.$router.push('/log-in')
                        })
                        .catch(error =>{
                            if(error.response){
                                for(const property in error.response.data){
                                    this.errors.push(`${property}:${error.response.data[property]}`)
                                }
                            }
                            else if (error.message){
                                this.errors.push('Something went wrong, please try again')
                            }
                        })
                }
            }
        }
    }
</script>