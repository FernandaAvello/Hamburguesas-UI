<template>
  <div class="container">
    <!-- Nav -->
    <div>
      <ul class="nav justify-content-between pb-2">
        <li class="nav-item">
          <h3 id="titulo">Tabla de Hamburguesas</h3>
        </li>

        <!-- Botón Crear -->
        <span class="d-flex justify-content-end">
          <li class="nav-item pe-3">
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#formularioCreacion"
            >
              Crear<font-awesome-icon class="ms-3" icon="plus" />
            </button>
          </li>

          <!-- Input Search -->
          <li class="nav-item">
            <div class="input-group mb-3">
              <input
                class="form-control mr-sm-2"
                type="search"
                placeholder="Ingrese ID o Nombre"
                v-model="palabraBuscada"
              />
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
          <th scope="col" class="col-md-4 subtitle">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="hamburguesa in hamburguesasFiltradas" :key="hamburguesa.id">
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
              data-bs-toggle="modal"
              data-bs-target="#modalButtonVer"
              title="Ver Detalles"
              @click="verDetalles(hamburguesa)"
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

    <FormularioHamburguesa />

    <!-- Modal Botón Ver-->
    <div class="modal fade" id="modalButtonVer" tabindex="-1">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="modalButtonVerLabel">Detalles</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <!-- Lista -->

            <ul class="list-group">
              <li class="list-group-item">
                <strong>Ingredientes:</strong>
                {{
                  hamburguesaSeleccionada.ingredientes
                    .toString()
                    .replaceAll(",", ", ")
                }}
              </li>
              <li class="list-group-item">
                <strong>Calorías:</strong>
                {{ hamburguesaSeleccionada.calorias }}
              </li>
            </ul>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Cerrar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>




<script>
import FormularioHamburguesa from "@/components/FormularioHamburguesa.vue";

export default {
  components: {
    FormularioHamburguesa,
  },

  data() {
    return {
      listaDeHamburguesas: [],
      hamburguesaSeleccionada: {
        id: null,
        ingredientes: [],
        nombre: "",
        calorias: null,
      },
      palabraBuscada: "",
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

    verDetalles(burger) {
      this.hamburguesaSeleccionada = burger;
    },
  },

  created() {
    this.getHamburguesas();
  },

  computed: {
    hamburguesasFiltradas() {
      return this.listaDeHamburguesas.filter((burger) => {
        return (
          burger.nombre
            .toLowerCase()
            .includes(this.palabraBuscada.toLowerCase()) ||
          burger.id == this.palabraBuscada
        );
      });
    },
  },
};
</script>


<style>
.crud-button {
  width: 10%;
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

.list-group-item {
  text-align: left;
}
</style>