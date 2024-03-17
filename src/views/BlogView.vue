<template>
  <div>
    <h1>Los Usuarios:</h1>
    <table class="user-table">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Correo</th>
          <th>Grupo</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.email">
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.group }}</td>
          <td>{{ user.password }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import UserService from '@/services/AuthService'
import type IUser from '@/interfaces/IUser'

const users = ref<IUser[]>([])

onMounted(async () => {
  try {
    users.value = await UserService.GetUsers()
  } catch (error) {
    console.error('No se pudieron recuperar los usuarios:', error)
  }
})
</script>

<style scoped>
.user-table {
  width: 100%;
  border-collapse: collapse;
}

.user-table th, .user-table td {
  padding: 8px;
  border: 1px solid #ddd;
}

.user-table th {
  background-color: #f2f2f2;
  text-align: left;
}
</style>


<!-- import bcrypt from 'bcryptjs';

async function loginUser() {
  try {
    const user = await UserService.GetEmail(email.value);

    if (user) {
      const passwordMatch = await bcrypt.compare(password.value, user.password);

      if (passwordMatch) {
        console.log('Usuario autenticado:', user);
      } else {
        console.error('Credenciales inválidas');
      }
    } else {
      console.error('Usuario no encontrado');
    }
  } catch (error) {
    console.error('No se pudo iniciar sesión:', error);
  }
} -->