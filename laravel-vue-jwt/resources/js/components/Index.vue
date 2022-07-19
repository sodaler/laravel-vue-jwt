<template>
    <div>
        <router-link :to="{ name: 'fruit.index' }">List</router-link>
        <router-link v-if="!accessToken" :to="{ name: 'user.login' }">Login</router-link>
        <router-link v-if="!accessToken" :to="{ name: 'user.registration' }">Registration</router-link>
        <router-link v-if="accessToken" :to="{ name: 'user.personal' }">Personal</router-link>
        <a href="#" v-if="accessToken" @click.prevent="logout">Logout</a>
        <router-view></router-view>
    </div>
</template>

<script>
import API from '../api'
export default {
    name: "Index",

    data() {
        return {
            accessToken: null
        }
    },

    mounted() {
        this.getAccessToken()
    },

    updated() {
        this.getAccessToken()
    },

    methods: {
        getAccessToken() {
            this.accessToken = localStorage.getItem('access_token')
        },
        logout() {
            API.post('/api/auth/logout')
            .then( res => {
                localStorage.removeItem('access_token')
                this.$router.push({ name: 'user.login' })
            })
        }
    }
}
</script>

<style scoped>

</style>
