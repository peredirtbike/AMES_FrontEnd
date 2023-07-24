<template>
  <div>
    <form @submit.prevent="saveClient" class="p-fluid">
      <div class="p-field">
        <label for="name">Nom:</label>
        <InputText v-model="client.name" id="name" required />
      </div>

      <div class="p-field">
        <label for="lastName">Cognoms:</label>
        <InputText v-model="client.lastName" id="lastName" required />
      </div>

      <div class="p-field">
        <label for="phone">Telèfon:</label>
        <InputText v-model="client.phone" id="phone" required />
      </div>

      <div class="p-field">
        <label for="email">Email:</label>
        <InputText type="email" v-model="client.email" id="email" required />
      </div>

      <div class="p-field">
        <label for="address">Adreça:</label>
        <InputText v-model="client.address" id="address" required />
      </div>

      <div class="p-field-checkbox">
        <Checkbox v-model="client.preferential" id="preferential" binary="true" />
        <label for="preferential">Preferencial</label>
      </div>

      <div class="p-field-checkbox">
        <Checkbox v-model="client.hasDiscount" id="discount" binary="true" />
        <label for="discount">Descompte</label>
      </div>

      <Button type="submit" :label="$route.params.id ? 'Actualizar' : 'Crear'" />
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