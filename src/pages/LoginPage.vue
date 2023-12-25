<template>
    <div>
        <div class="row align-center justify-center column">
            <label>Nazwa użytkownika</label>
            <input type="text" v-model="username" />

            <label>Hasło</label>
            <input type="password" v-model="password" />
        </div>

        <q-btn class="q-mt-sm" @click="authorization">Zaloguj</q-btn>
    </div>
</template>

<script setup>
import axios from 'axios'
import { ref } from "vue";
import { useRouter } from 'vue-router'

const router = useRouter()
const connectWithApi = import.meta.env.VITE_LOCAL || "http://ec2-13-51-107-19.eu-north-1.compute.amazonaws.com:3000/"
const username = ref('')
const password = ref('')

const authorization = () => {
    const result = {
        username: username.value,
        password: password.value
    }

    axios.post(`${connectWithApi}api/login`, { result })
        .then((response) => {
            if (response.data) {
                router.push({ path: '/home' })
            } else {
                alert('Błędne dane do logowania')
            }
        });
}

</script>