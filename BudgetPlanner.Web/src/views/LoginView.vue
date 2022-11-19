<template>
  <span>{{ show }}</span>
    <b-container>
        <b-row cols="4" align-h="center">
            <b-form @submit="onSubmit" v-if="show">
                <b-form-group
                id="input-group-1"
                label="Email:"
                label-for="input-1"
                >
                <b-form-input
                    id="input-1"
                    v-model="form.email"
                    type="email"
                    placeholder="Email"
                    required
                ></b-form-input>
                </b-form-group>
        
                <b-form-group id="input-group-2" label="Password:" label-for="input-2">
                <b-form-input
                    id="input-2"
                    v-model="form.password"
                    type="password"
                    placeholder="Password"
                    required
                ></b-form-input>
                </b-form-group>
                <b-row class="mt-4" cols="2" align-h="center">
                    <b-button type="submit">Login</b-button>
                </b-row>
                <!-- <b-row class="mt-3">
                    <b-link class="text-center">Reset password</b-link>
                </b-row> -->

                
            </b-form>
        </b-row>
    </b-container>
  </template>
  
  <script lang="ts">
    type Form = {
      email: string;
      password: string;
    }
    let show = true
    let form: Form = {
      email: '',
      password: ''
    }

    export default {
      data() {
        return {
          form,
          show
        }
      },
      methods: {
        onSubmit() {
          console.log(form);
          fetch('https://localhost:7269/api/auth/login', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              email: form.email,
              password: form.password
            })
          })
            .then(response => response.json())
            .then(data => {
              fetch('https://localhost:7269/api/weatherforecast', {
                method: 'GET',
                headers: {
                  'Authorization': 'Bearer ' + data.token,
                  'Content-Type': 'application/json'
                }
              })
              .then(resp => resp.json())
              .then(x => alert('Hello ' + data.firstName + '! The weather is: ' + JSON.stringify(x)))
            })
        }
      }
    }
  </script>