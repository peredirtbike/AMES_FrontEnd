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

      <Button type="submit">{{ $route.params.id ? 'Actualizar' : 'Crear' }}</Button>
      
    </form>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';


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
        preferential: false,
        hasDiscount: false,
      },
    };
  },
  computed: {
    ...mapGetters(['getClientById']),
    isUpdating() {
      return !!this.$route.params.id; 
    },
  },
  methods: {
    ...mapActions(['fetchClientData', 'updateClient', 'createClient']),
    saveClient() {
      const action = this.isUpdating ? this.updateClient : this.createClient;
      action(this.client)
        .then(response => {
          console.log(this.isUpdating ? 'Cliente actualizado:' : 'Nuevo cliente creado:', response.data);
          this.$router.push('/clients');
        })
        .catch(error => {
          console.error(this.isUpdating ? 'Error al actualizar el cliente:' : 'Error al crear el cliente:', error);
        });
    },
    // Resto de los métodos
  },
  mounted() {
    if (this.$route.params.id) {
      const clientId = this.$route.params.id;
      this.fetchClientData(clientId).then(client => {
        this.client = client;
      });
    }
  },
};
</script>