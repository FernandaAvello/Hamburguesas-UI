<template>
  <div class="container">
    <!-- Nav -->
    <div>
      <ul class="nav justify-content-between pb-2">
        <li class="nav-item">
          <h3 id="titulo">Tabla de Hamburguesas</h3>
        </li>
        <!-- Botón Añadir -->
        <span class="d-flex justify-content-end">
          <li class="nav-item pe-5">
            <button type="button" class="btn btn-primary">
              Crear nueva hamburguesa<font-awesome-icon class="ms-3" icon="plus"/>
            </button>
          </li>

          <!-- Input Search -->
          <li class="nav-item">
            <div class="input-group mb-3">
              <input
                class="form-control mr-sm-2 w-50"
                type="search"
                placeholder="Ingrese ID o Nombre"
              />
              <button
                class="btn btn-outline-secondary my-2 my-sm-0"
                type="submit"
              >
                Buscar
              </button>
            </div>
          </li>
        </span>
      </ul>
    </div>

    <!-- Table -->
    <table class="table table-bordered table-responsive">
      <thead class="table-light">
        <tr>
          <th scope="col" class="subtitle">#ID</th>
          <th scope="col" class="subtitle">Nombre</th>
          <th scope="col" class="subtitle">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="hamburguesa in listaDeHamburguesas" :key="hamburguesa.id">
          <th scope="row">{{ hamburguesa.id }}</th>
          <td>{{ toCapitalFirst(hamburguesa.nombre) }}</td>
          <td>
            <!-- Botón Editar -->
            <button
              type="button"
              class="btn btn-warning crud-button"
              data-bs-toggle="tooltip"
              data-bs-placement="bottom"
              title="Editar"
            >
              <font-awesome-icon icon="pencil" />
            </button>

            <!-- Botón Ver -->
            <button
              type="button"
              class="btn btn-success crud-button"
              data-bs-toggle="tooltip"
              data-bs-placement="bottom"
              title="Ver Detalles"
            >
              <font-awesome-icon icon="eye" />
            </button>

            <!-- Botón Borrar -->
            <button
              type="button"
              class="btn btn-danger crud-button"
              data-bs-toggle="tooltip"
              data-bs-placement="bottom"
              title="Eliminar"
            >
              <font-awesome-icon icon="trash-can" />
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      listaDeHamburguesas: [],
    };
  },

  methods: {
    getHamburguesas() {
      // Buscar la URL
      this.$http
        .get("https://prueba-hamburguesas.herokuapp.com/burger/")
        .then((response) => {
          this.listaDeHamburguesas = response.data;
        });
    },

    toCapitalFirst(nombre) {
      return nombre.charAt(0).toUpperCase() + nombre.slice(1);
    },
  },

  created() {
    this.getHamburguesas();
  },
};
</script>


<style>
.crud-button {
  width: 7%;
  height: 7%;
  /* color: #fff; */
  margin-right: 15px;
}

#titulo {
  font-weight: 700;
  font-size: 2rem;
}

.subtitle {
  font-weight: 700;
  font-size: 1.2rem;
}
</style>