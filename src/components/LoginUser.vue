<script setup lang="ts">
import InputGroup from 'primevue/inputgroup'
import InputGroupAddon from 'primevue/inputgroupaddon'
import InputText from 'primevue/inputtext'
import FloatLabel from 'primevue/floatlabel'
import Button from 'primevue/button'
import { useToast } from 'primevue/usetoast'
import axios from 'axios'
import { computed, ref } from 'vue'
import type { User } from '@/types/user'
import store from '@/store'

interface LoginResponse {
  user: User
  access_token: string
  refresh_token: string
}

const username = ref<string>('')
const password = ref<string>('')
const isLoginDisabled = computed(() => !username.value || !password.value)

const errorMessage = ref<string>('')

const toast = useToast()

const handleLogin = async () => {
  try {
    const response = await axios.post<LoginResponse>('http://localhost:8080/v1/login-user', {
      username: username.value,
      password: password.value,
    })
    alert(response)

    store.setUser(response.data.user, response.data.access_token, response.data.refresh_token)
  } catch (error: any) {
    if (error.response) {
      errorMessage.value = `An error ocured ${error.response.data.message}`
      toast.add({ severity: 'error', summary: 'hello', detail: 'message value', life: 3000 })
    }
  }
}
</script>

<template>
  <div class="flex flex-column row-gap-5">
    <InputGroup>
      <InputGroupAddon>
        <i class="pi pi-user"></i>
      </InputGroupAddon>
      <FloatLabel>
        <InputText id="Username" v-model="username" />
        <label for="Username">Username</label>
      </FloatLabel>
    </InputGroup>

    <InputGroup>
      <InputGroupAddon>
        <i class="pi pi-lock"></i>
      </InputGroupAddon>
      <FloatLabel>
        <InputText type="password" id="Password" v-model="password" />
        <label for="Password">Password</label>
      </FloatLabel>
    </InputGroup>
    <Button label="Login" :disabled="isLoginDisabled" @click="handleLogin" />
  </div>
</template>