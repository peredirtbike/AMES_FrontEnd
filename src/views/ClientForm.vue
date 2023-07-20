<template>
    <div>
      <form @submit.prevent="saveClient">
        <label for="name">Nom:</label>
        <input type="text" id="name" v-model="client.name" required>
  
        <label for="lastName">Cognoms:</label>
        <input type="text" id="lastName" v-model="client.lastName" required>
  
        <label for="phone">Telèfon:</label>
        <input type="text" id="phone" v-model="client.phone" required>

        <label for="email">Email:</label>
        <input type="email" id="email" v-model="client.email" required>

        <label for="address">Adreça:</label>
        <input type="text" id="address" v-model="client.address" required>

        <label for="preferential">Preferencial:</label>
        <input type="checkbox" id="preferential" v-model="client.preferential">

        <label for="discount">Descompte:</label>
        <input type="checkbox" id="discount" v-model="client.hasDiscount">
  
        <button type="submit">Guardar</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'ClientForm',
    data() {
      return {
        client: {
          name: '',
          lastName: '',
          phone: '',
          email: '',
          address: '',
          preferential: '',
          hasDiscount: '',
        },
      };
    },
    methods: {
      saveClient() {
        if (this.$route.params.id) {
          // Resto del código para actualizar cliente existente...
        } else {
          // Crear un nuevo cliente
          axios.post('http://localhost:8080/clients/newClient', this.client)
            .then(response => {
              console.log('Nuevo cliente creado:', response.data);
              // Redireccionar a la lista de clientes después de la creación
              this.$router.push('/clients');
            })
            .catch(error => {
              console.error('Error al crear el cliente:', error);
            });
        }
      },
    },
    mounted() {
      // Si hay un ID en la ruta, obtener los datos del cliente para editar
      if (this.$route.params.id) {
        axios.get(`http://localhost:8080/clients/${this.$route.params.id}`)
          .then(response => {
            this.client = response.data;
          })
          .catch(error => {
            console.error('Error al obtener los datos del cliente:', error);
          });
      }
    },
  };
  </script>