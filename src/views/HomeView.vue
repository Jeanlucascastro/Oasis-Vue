<script setup lang="ts">
import TheWelcome from '../components/TheWelcome.vue'
import axios from 'axios'
import { ref, onMounted } from 'vue'

const user = ref('')
const token = ref('')
const password = ref('')
const email = ref('')

const fetchData = async () => {
  try {
    await axios
      .post(
        'http://192.168.0.104:3001/api/auth/login',
        {
          email: "jean@gmail.com",
          password: "^X9$jP0#aBHxD5"
        },
        {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Headers': 'Origin, X-Requested-With, Content-Type, Accept'
          }
        }
      )
      .then((usuario) => {
        user.value = usuario.data.usuario
        token.value = usuario.data.token
        password.value = usuario.data.password
        email.value = usuario.data.email
        sessionStorage.setItem('token-oasis', usuario.data.token)
        sessionStorage.setItem('usuario-oasis', usuario.data.usuarioId)
        sessionStorage.setItem('company-oasis', usuario.data.companyId)

        console.log('company-oasis', usuario)
      })
  } catch (error) {
    console.error('Error fetching data:', error)
  }
}

onMounted(() => {
  fetchData()
})
</script>

<template>
  <main>
    <TheWelcome />
  </main>
</template>
