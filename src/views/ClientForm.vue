<template>
  <div class="flex card justify-content-center align-items-center" style="height: 100vh;">
    <div class="card p-4 shadow-8 border-round">

      <div class="flex align-items-center justify-content-between mb-4" >
        <span class="text-xl text-900 font-bold">{{ isUpdating ? 'Actualizar' : 'Crear' }} Client</span>
        <Button class="" @click="redirectToClientsPage">Tornar</Button>
      </div>
      <form @submit.prevent="saveClient" class="formgrid grid">
        <div class="field col-12 md:col-6 mb-3">
          <label for="name">Nom</label>
          <InputText v-model="client.name" id="name" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary w-full" required />
        </div>
        <div class="field col-12 md:col-6 mb-3">
          <label for="lastName">Cognoms</label>
          <InputText v-model="client.lastName" id="lastName" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary w-full" required />
        </div>
        <div class="field col-12 md:col-8 mb-3">
          <label for="address">Adreça</label>
          <InputText v-model="client.address" id="address" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary w-full" required />
        </div>
        <div class="field col-12 md:col-4 mb-3">
          <label for="phone">Telèfon</label>
          <InputText v-model="client.phone" id="phone" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary w-full" required />
        </div>
        <div class="field col-12 md:col-6 mr-4 mb-3">
          <label for="email">Email</label>
          <InputText v-model="client.email" id="email" class="text-base text-color surface-overlay p-2 border-1 border-solid surface-border border-round appearance-none outline-none focus:border-primary w-full" required />
        </div>

        <div class="card flex flex-wrap gap-4 ml-2">
          <div class="flex align-items-center">
            <Checkbox v-model="client.preferential" inputId="preferential" name="preferential" :binary="true" />
            <label for="preferential" class="ml-2">És un client preferencial?</label>
          </div>
          <div class="flex align-items-center">
            <Checkbox v-model="client.hasDiscount" inputId="hasDiscount" name="hasDiscount" :binary="true" />
            <label for="hasDiscount" class="ml-2">Té descompte?</label>
          </div>
        </div>

        <div class="flex field col-12 mt-4">
          <Button type="submit" :label="isUpdating ? 'Actualitzar' : 'Crear'" />
        </div>
      </form>
    </div>
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
          console.log(this.isUpdating ? 'Cliente actualizado' : 'Nuevo cliente creado', response);
          this.$router.push('/clients');
        })
        .catch(error => {
          console.error(this.isUpdating ? 'Error al actualizar el cliente:' : 'Error al crear el cliente:', error);
        });
    },

    redirectToClientsPage() {
      this.$router.push('/clients');
    }
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