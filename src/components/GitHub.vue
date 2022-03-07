<template>
    <div>
        <div>
            <!--<input type="text" placeholder="introduce usuario" v-model="user" @keydown.enter="obtenerUsuario">-->
            <input type="text" placeholder="introduce usuario"  v-model="user"> 
            <button type="button" @click="obtenerUsuario" class="btn btn-primary"> Buscar </button>
        </div>

        <div v-if="userValid">
            <img :src="userData.avatar_url" style="width:100px">
            <br>
            Mostrar el nombre: {{ userData. login }}
            <a :href="userData.html_url" target="_blank"> URL del {{ userData.login }} </a>
            <button type="button" class="btn btn-primary" @click="obtenerRepositorios"> Repositorios </button>
        </div>

        <div v-if="userRepoValid">
            <GitHubRepos :repolist="userRepo"></GitHubRepos>
        </div>
    </div>
</template>

<script>

import GitHubRepos from "./GitHubRepos.vue"

export default {
    name: 'GitHub',
    components: {
        GitHubRepos
    },
    data: function() {
        return {
            user: "",
            userData: {},
            userValid: false,
            userRepo: {},
            userRepoValid: false,
        }
    },
    methods: {
        obtenerUsuario: function() {
            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();
            headers.set('Authorization', 'Basic ' + btoa(userAuth + ":" + passAuth));

            var url = "https://api.github.com/users/" + this.user;

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.userData = response;
                this.userValid = true;
            })
            .catch(err => {
                console.log(err);
            })
        },
        obtenerRepositorios: function() {
             var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();
            headers.set('Authorization', 'Basic ' + btoa(userAuth + ":" + passAuth));

            var url = this.userData.repos_url;

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.userRepo = response;
                this.userRepoValid = true;
            })
            .catch(err => {
                console.log(err);
            })
        }
    }
}
</script>

<style scoped>
</style>
