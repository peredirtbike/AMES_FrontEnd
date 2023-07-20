<template>
  <div id="app">
    <!-- Aquí va el contenido común a todas las vistas, como encabezado, barra de navegación, etc. -->

    <!-- El componente router-view mostrará la vista asociada a la ruta actual -->
    <router-view></router-view>

    <!-- Aquí va el contenido común a todas las vistas, como pie de página, etc. -->
  </div>
</template>

<script>
// Importa Vue Router para poder utilizarlo en App.vue
import { useRouter } from 'vue-router';

export default {
  name: 'App',
  setup() {
    // Obtiene la instancia de Vue Router para utilizarla en el setup de la aplicación
    const router = useRouter();

    return {
      router 
    };
  }
};
</script>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
