<template>
    <div>
        <h2>Iniciar Sesión</h2>
        <input v-model="username" placeholder="Usuario" />
        <input v-model="password" type="password" placeholder="Contraseña" />
        <button @click="login">Acceder</button>
    </div>
</template>

<script>
import axios from '../axios';

export default {
    data() {
        return {
            username: '',
            password: ''
        };
    },
    methods: {
        async login() {
            try {
                const response = await axios.post('/login', {
                    username: this.username,
                    password: this.password
                });

                localStorage.setItem('token', response.data.token);
                axios.defaults.headers.common['Authorization'] = `Bearer ${response.data.token}`;

                this.$router.push('/reportList');
            } catch (error) {
                alert('Credenciales incorrectas');
            }
        }
    }
};
</script>