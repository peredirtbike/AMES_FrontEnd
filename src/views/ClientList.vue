<template>
  <div>
    <table>
      <thead>
        <tr>
        <td colspan="8" class="center">
          <router-link to="/clients/new"> <!-- Redirige a /clients/new -->
            <button>New Client</button>
          </router-link>
        </td>
      </tr>
        <tr>
          <th>Nom</th>
          <th>Cognom</th>
          <th>Telèfon</th>
          <th>Email</th>
          <th>Adreça</th>
          <th>Preferencial</th>
          <th>Descompte</th>
          <th>Accions</th>
        </tr>
      </thead>
      <div>
        <input type="text" v-model="searchTerm" placeholder="Buscar clientes...">
      </div>
      <tbody>
        <tr v-for="client in filteredClients" :key="client.id">
          <td>{{ client.name }}</td>
          <td>{{ client.lastName }}</td>
          <td>{{ client.phone }}</td>
          <td>{{ client.email }}</td>
          <td>{{ client.address }}</td>
          <td>{{ client.preferential }}</td>
          <td>{{ client.hasDiscount }}</td>
          <td>
            <button @click="editClient(client.id)">Editar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ClientList',
  data() {
    return {
      clients: [],
      searchTerm: '',
    };
  },
  methods: {
    fetchClients() {
      axios.get('http://localhost:8080/clients/getAllClients')
        .then(response => {
          this.clients = response.data;
        })
        .catch(error => {
          console.error('Error al obtener los clientes:', error);
        });
    },
    editClient(clientId) {
      // Redireccionar a la vista de edición de cliente con el ID
      this.$router.push(`/clients/${clientId}`);
    },
  },
  computed: {
  filteredClients() {
    return this.clients.filter(client => {
      return (
        client.name.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
        client.lastName.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
        client.phone.includes(this.searchTerm) ||
        client.email.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
        client.address.toLowerCase().includes(this.searchTerm.toLowerCase())
      );
    });
  },
},
  mounted() {
    this.fetchClients();
  },
};
</script>