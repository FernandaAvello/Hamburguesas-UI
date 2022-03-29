
<template>
  <div class="container">
   
    <!-- Nav -->

    <div>
      <ul class="nav justify-content-between pb-2">
        <li class="nav-item d-inline-flex flex-row">
          <h3 id="titulo"><strong>Tabla de Hamburguesas</strong></h3>
          <div id="img-hamburguesa"></div>
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

          <!-- Input Buscar -->

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
    <br>

    <!-- Table -->

    <table class="table table-bordered table-responsive">
      <thead class="table-light">
        <tr>
          <th scope="col" class="subtitle">#ID</th>
          <th scope="col" class="subtitle">Nombre</th>
          <th scope="col" class="col-md-4 subtitle">Acciones</th>
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
              data-bs-placement="bottom"
              title="Editar"
              @click="editarHamburguesa(hamburguesa)"
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
              data-bs-placement="bottom"
              title="Eliminar"
              @click="abrirModalBorrar(hamburguesa)"
            >
              <font-awesome-icon icon="trash-can" />
            </button>
          </td>
        </tr>
      </tbody>
    </table>


     <!-- Modal Botón Crear -->

  <div
    class="modal fade"
    id="formularioCreacion"
    tabindex="-1"
    ref="formularioCreacion"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="formularioCreacionLabel">
            <strong>Crea tu Hamburguesa</strong>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <FormularioHamburguesa :cerrarModal="cerrarModalCrear" :limpiar="ejecutarLimpieza" />
        </div>
      </div>
    </div>
  </div>

 <!-- Modal Botón Editar -->

  <div
    class="modal fade"
    id="formularioEdit"
    tabindex="-1"
    ref="formularioEdit"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="formularioEditLabel">
            <strong>Edita tu Hamburguesa</strong>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <FormularioEdit
            :hamburguesa="hamburguesaSeleccionada"
            :cerrarModal="cerrarModalEditar"
          />
        </div>
      </div>
    </div>
  </div>

    <!-- Modal Botón Ver-->

    <div class="modal fade" id="modalButtonVer" tabindex="-1">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="modalButtonVerLabel"><strong>Detalles</strong></h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">

            <!-- Lista de Ingredientes en Modal Ver -->

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

  <!-- Modal Botón Borrar -->

  <div
    class="modal fade"
    id="modalButtonBorrar"
    tabindex="-1"
    ref="modalButtonBorrar"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="confirmacionBorrarLabel"><strong>Eliminar Hamburguesa</strong></h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">¿Estás seguro que deseas borrar la hamburguesa seleccionada?</div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            No
          </button>
          <button type="button" class="btn btn-primary" @click="eliminarHamburguesa">Sí</button>
        </div>
      </div>
    </div>
  </div>
</template> 


<script>
// Imports de los Componentes
import { Modal } from "bootstrap";
import FormularioHamburguesa from "@/components/FormularioHamburguesa.vue";
import FormularioEdit from "@/components/FormularioEdit.vue";

export default {
  components: {
    //Declaración de los Componentes
    FormularioHamburguesa,
    FormularioEdit,
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
      modalCrear: null,
      modalEditar: null,
      modalBorrar: null,
      ejecutarLimpieza: false,
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

    toCapitalFirst(nombre) { // Se busca que al mostrar el nombre de una hamburguesa en la tabla, siempre aparezca con mayuscula al principio.
      return nombre.charAt(0).toUpperCase() + nombre.slice(1);
    },

    verDetalles(burger) {
      this.hamburguesaSeleccionada = burger;
    },

    crearHamburguesa() {
      this.ejecutarLimpieza = false
      this.modalCrear.show();
    },

    editarHamburguesa(hamburguesa) {
      this.hamburguesaSeleccionada = hamburguesa;
      this.modalEditar.show();
    },

    cerrarModalCrear() {
      this.ejecutarLimpieza = true
      this.modalCrear.hide();
      this.getHamburguesas();
    },

    cerrarModalEditar() {
      this.modalEditar.hide();
      this.getHamburguesas();

    },

    eliminarHamburguesa() {
     this.$http
        .delete(`https://prueba-hamburguesas.herokuapp.com/burger/${this.hamburguesaSeleccionada.id}`)
        .then((response) => {
        console.log(response);
        this.modalBorrar.hide();
        this.getHamburguesas();
        })
    },

    abrirModalBorrar(hamburguesa) {
      this.hamburguesaSeleccionada = hamburguesa
      this.modalBorrar.show();
    }

  },

  created() {
    this.getHamburguesas();
  },

  mounted() { //Se crean instancias de los modales para cerrar y abrirlos via JS
    this.modalCrear = new Modal(this.$refs.formularioCreacion);
    this.modalEditar = new Modal(this.$refs.formularioEdit);
    this.modalBorrar = new Modal(this.$refs.modalButtonBorrar)
  },

  computed: {
    hamburguesasFiltradas() { // Se hace este computed property para filtrar la tabla de hamnburguesas segun la palabra escrita en el input de "Buscar"
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
#img-hamburguesa {
  background-image: url("../assets/hamburguesa.png");
  background-size: contain;
  height: 50px;
  width: 50px;
}
.crud-button {
  height: 7%;
  margin-right: 15px;
}

#titulo {
  font-weight: 700;
  font-size: 2rem;
  padding-right: 10px;
}

.subtitle {
  font-weight: 700;
  font-size: 1.2rem;
}

.list-group-item {
  text-align: left;
}
</style>