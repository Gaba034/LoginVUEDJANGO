<template>
    <v-app  style="background-color:#1D6382" view="lHh Lpr lFf">
      <v-app-bar
      flat
      outlined
      color="#1D6382"
      >
      <div class="btn-container">
        <div id="app">
          <nav>
            <router-link to="/">Home</router-link> |
            <router-link to="/about">About</router-link>
          </nav>
          <router-view/>
        </div>
        <div >
          <v-btn class="v-btn btn-text">
           Cadastro
          </v-btn>
        </div>
            <v-divider 
            dark
            vertical
            inset
            class="mx-4"
            >
            </v-divider>
            <div>
              <v-btn class="v-btn">
                <v-row justify="space-around">
            <v-col cols="auto">
            </v-col>
            <v-col cols="auto">
              <v-dialog
                transition="dialog-top-transition"
                max-width="600"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="primary"
                    v-bind="attrs"
                    v-on="on"
                  >From the top</v-btn>
                </template>
                <template v-slot:default="dialog">
                  <v-card>
                    <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Email</label>
                        <div class="control">
                            <input type="email" name="email" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input type="password" name="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Submit</button>
                        </div>
                    </div>
                </form>
                  </v-card>
                </template>
              </v-dialog>
            </v-col>
            </v-row>
          </v-btn>
        </div>
      </div>
        <div class="spheres"
        >
          <div class="sphere"></div>
          <div class="sphere"></div>
          <div class="sphere">
            <div class="image"></div>
          </div>
          <div class="sphere"></div>
          <div class="sphere"></div>
          <div class="sphere"></div>
  
        </div>
      </v-app-bar>
      <v-container>
        <div 
        style="
        background-color:#1D6382;
        padding-top:10%;
    
  
        "
        class="text-container "
        >
        <div class="sphere-cube"></div>
        <div class="cube"></div>
        <div class="cube">
          <div class="image1"></div>
        </div>
        <div class="text">
          <div class="text text-shadow">
            Controle de Acesso a Fábrica de Software
          </div>
          <div class="text text-shadow">
            O Controle de acesso 
            à fábrica de software, 
            referido pelo acrônimo C.A.F.S, 
            é um sistema que busca auxiliar 
            os coordenadores e as suas equipes
            a gerenciarem a sua jornada de trabalho 
            previamente estabelecida
          </div>
        </div>
      </div>
      </v-container>
    </v-app>
  </template>
<script>
    import axios from 'axios'

    export default {
        name: 'LogIn',
        data() {
            return {
                username: '',
                password: '',
                errors: []
            }
        },
        methods: {
            async submitForm() {
                this.$store.commit('setIsLoading', true)

                axios.defaults.headers.common['Authorization'] = ''
                localStorage.removeItem('token')

                const formData = {
                    username: this.username,
                    password: this.password
                }

                await axios
                    .post('/api/v1/token/login/', formData)
                    .then(response => {
                        const token = response.data.auth_token

                        this.$store.commit('setToken', token)

                        axios.defaults.headers.common['Authorization'] = 'Token ' + token

                        localStorage.setItem('token', token)
                    })
                    .catch(error => {
                        if (error.response) {
                            for (const property in error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }
                        } else if (error.message) {
                            this.errors.push('Something went wrong. Please try again!')
                        }
                    })

                await axios
                    .get('/api/v1/users/me')
                    .then(response => {
                        this.$store.commit('setUser', {'id': response.data.id, 'username': response.data.username})

                        localStorage.setItem('username', response.data.username)
                        localStorage.setItem('userid', response.data.id)
                    })
                    .catch(error => {
                        console.log(error)
                    })

                await axios
                    .get('/api/v1/teams/get_my_team/')
                    .then(response => {
                        this.$store.commit('setTeam', {'id': response.data.id, 'name': response.data.name})

                        this.$router.push('/dashboard/my-account')
                    })
                    .catch(error => {
                        console.log(error)
                    })
                
                this.$store.commit('setIsLoading', false)
            }
        }
    }
</script>
<style>
  @viewport {
  user-zoom: fixed;
}
.sidebar-content{
  height: 100%;
}
.title{
  color: white;
  font-size: 40px;
  font-weight: 500;
}
.minor-title{
  color: white;
}
.container{
  height: 93vh;
  
}
.btn-container{
  display: flex;
  height: 100%;
  width: 100%;
  justify-content: right;
  padding: 10px;
  
}
.v-btn:before {
    content: "";
    display: block;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    border-radius: inherit;
    box-shadow:none;
}

.cube:nth-child(3){
  background-color: #FFFFFF;
  width: 36vw;
  height: 70vh;
  border-radius: 30px;
  transform: rotate(45deg);
  position: fixed;
  left: 12vw;
  top:50vh;
  display:flex;
  box-shadow: 0px 10px 30px #3b3b3b57;
  border-right: rgba(0, 0, 0, 0.397) 1px solid;
  overflow:hidden;
  justify-content: center;
  align-content: center;
  align-items: center;
}
.cube:nth-child(2){
  background-color: #D9D9D9;
  width: 37vw;
  height: 70vh;
  border-radius: 40px;
  transform: rotate(45deg);
  position: fixed;
  left: 3vw;
  top: 45vh;
  box-shadow: 0px 10px 30px #3b3b3b57;
}
.sphere-cube{
  background-color: #5A8A9F; 
  width: 9vw;
  height: 9vw;
  border-radius: 100px;
  position:relative;
  left: -25vw;
  top: 5vh;
}
.sphere:nth-child(6){
  background-color: #5A8A9F; 
  width: 9vw;
  height: 9vw;
  border-radius: 100px;
  position:relative;
  right: 10vw;
  top: 67vh;
}
.sphere:nth-child(5){
  background-color: #5a8a9f81; 
  width: 6vw;
  height: 6vw;
  border-radius: 100px;
  position:relative;
  right: 8vw;
  top:73vh;
}
.sphere:nth-child(4){
  background-color: #5a8a9fb6; 
  width: 6vw;
  height: 6vw;
  border-radius: 100px;
  position:fixed;
  left: 350px;
  top: 65px;
}
.sphere:nth-child(3){
  background-color: #6e9eb2
  ; 
  width: 9vw;
  height: 9vw;
  border-radius: 100px;
  position:absolute;
  left: 3.7vw;
  top: 5vh;
  overflow: hidden;
  display: flex;
  padding-top: 1%;
  justify-content: space-around;
  align-content:space-around;
}
.sphere:nth-child(3) .image{
  width: 60%;
  height: auto;
  background-size: contain;
  border-radius: 10px;
}
.sphere:nth-child(2){
  background-color: #5a8a9f9f; 
  width: 6vw;
  height: 6vw;
  border-radius: 100px;
  position:absolute;
  left: 4vh;
  top: 11vh;
}
.sphere:nth-child(1){
  background-color: #5a8a9f81; 
  width: 6vw;
  height: 6vw;
  border-radius: 100px;
  position:absolute;
  left: 9vw;
  top: 6vh;
}
.spheres{
  width: 0;
  height: 0;
}
.text{
  width: 60vh;
  height: 26vh;
  background-color:#1d6482ea;
  border-radius: 20px 0px 10px 20px;
}
.text:nth-child(1){
  font-size: 3em;
  line-height: 100%;
  text-align:right;
  position: absolute;
  top: 30vh;
  right: 10vh;
}
.text:nth-child(2){
  font-size: 1.75em;
  line-height: 100%;
  text-align:right;
  position: absolute;
  right: 10vh;
  top: 50vh;
}
html{
  overflow: hidden;
}
.text{
  color: white;
}
.text-container{
  height: 100%;
  width: 100%;
  justify-content: flex-end;
  display: flex;
}
.sidebar{
  background-color: black;
}
</style>