<template>
  <div class="user-login">
    <h1>Iniciar sesión</h1>
    <form @submit.prevent="loginUser" class="login-form">
      <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required>
      </div>
      <div class="form-group">
        <label for="password">Contraseña:</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <button type="submit" class="submit-button">Iniciar sesión</button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import UserService from '@/services/AuthService'
import bcrypt from 'bcryptjs'

let email = ref('')
let password = ref('')

async function loginUser() {
  try {
    const user = await UserService.GetEmail(email.value, password.value);

    if (user && user.password) { // Verificar que user y user.password estén definidos
      const passwordMatch = await bcrypt.compare(password.value, user.password);

      if (passwordMatch) {
        console.log('Usuario autenticado:', user);
      } else {
        console.error('Credenciales inválidas');
      }
    } else {
      console.error('Usuario no encontrado o contraseña no válida');
    }
  } catch (error) {
    console.error('No se pudo iniciar sesión:', error);
  }
}

</script>

<style scoped>
/* Estilos CSS para el formulario de inicio de sesión */
</style>
