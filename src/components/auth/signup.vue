<template>
<div id="signup">
    <div id="signin">
        <div class="form">
            <form class="login-form" @submit.prevent="onSubmit">
                <div class="input" :class="{invalid: $v.email.$error}">
                    <h6 v-if="!$v.email.email">Please provide a valid email address</h6>
                    <h6 v-if="!$v.email.unique">This email address has been taken</h6>
                <input type="email" placeholder="Email" id="email" @blur="$v.email.$touch()" v-model="email">
                <!-- <p> {{$v}} -->
                </div>
                <div class="input">
                    <input type="name" placeholder="Name" id="name" v-model="name">
                     <!-- <p> {{$v}} -->
                </div>
                <div class="input" :class="{invalid:$v.age.error}">
                    <h6 v-if="!$v.age.minVal">You have to be at least {{$v.age.$params.minVal.min}}</h6>
                    <input type="number" id="age" placeholder="Age" @blur="$v.age.$touch()" v-model.number="age">
                </div>
                <div class="input" :class="{$invalid:$v.password.$error}">
                    <input type="password" placeholder="Password" id="password" @blur="$v.password.$touch()" v-model="password">
                </div>
                 <div class="input" :class="{$invalid:$v.confirmPassword.$error}">
                    <input type="password" placeholder="Confirm Password" id="confirm-password" @blur="$v.confirmPassword.$touch()" v-model="confirmPassword">
                </div>
                <button type="submit" :disabled="$v.$invalid"> Create </button>
            </form>
        </div>
    </div>
</template>
<script>
import { required, email, numeric, minValue, minlength, sameAs } from 'vuelidate/lib/validators';
import axios from 'axios';
export default {
    validations: {
        email: {
            required,
            email,
            unique: val => {
                if(val == '') return true
                return axios.get()
                .then(res => {
                    return Object.keys(res.data).length === 0
                })
            }
        },
        age: {
            required,
            numeric,
            minVal: minValue(18)
        },
        password: {
            required,
            minlen: minlength(6)
        },
        confirmPassword: {
            sameAs: sameAs(vm => {
                return vm.password
            })
        }
    },

    state: {
        idToken: null,
        userId: null,
        user: null
    },
    mutations: {
        authUser(state, userData) {
            state.idToken = userData.token
            state.userId = userData.userId
        },
        // storeUser(state, user) {

        // }
    }
    
}
</script>

