<template>
  <div>
    <div class="card shadow-sm p-6">
      <div class="card-header">
        <div>
          <button
            class="btn btn-light-primary gap-2"
            data-bs-toggle="modal"
            data-bs-target="#kt_add_Puesto_modal"
          >
            <i class="bi bi-plus fs-1"
              ><span class="path1"></span><span class="path2"></span
              ><span class="path3"></span><span class="path4"></span
            ></i>
            Agregar Puesto
          </button>
        </div>
      </div>
      <div class="py-5">
        <div class="table-responsive">
          <table class="table table-row-dashed table-row-gray-300 gy-7">
            <thead>
              <tr class="fw-bold fs-6 text-gray-800">
                <th>Puesto</th>
                <th>Descripcion</th>
                <th>Salario</th>
                <th>Departamento</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(puesto, index) in Puestos">
                <td>{{ puesto.Nombre }}</td>
                <td class="mw-100px">{{ puesto.Descripcion }}</td>
                <td class="text-success">{{ puesto.Salario }}</td>
                <td class="text-primary">
                  {{ getNameDepartamento(puesto.idDepartamento) }}
                </td>
                <td>
                  <div class="d-flex flex-shrink-0">
                    <a
                      class="btn btn-icon btn-bg-light btn-active-color-primary btn-sm me-1"
                      @click="EditarPuesto(puesto.idDepartamento)"
                    >
                      <i class="bi bi-pencil-fill fs-3"></i>
                  </a>
                    <a
                      @click="EliminarPuesto(puesto.idDepartamento)"
                      class="btn btn-icon btn-bg-light btn-active-color-danger btn-sm"
                      data-bs-toggle="modal"
                      data-bs-target="#kt_delete_Puesto_modal"
                    >
                      <i class="bi bi-trash-fill fs-3"></i>
                    </a>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
  <AddPuesto :refresh="getPuestos" />
  <editPuesto v-if="idPuesto"  :refresh="getPuestos" :id="idPuesto" />

</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import { apiApp } from "@/core/api/apiApp";
import AddPuesto from "@/components/modals/forms/AddPuesto.vue";
import editPuesto from "@/components/modals/forms/EditPuesto.vue";
import { Modal } from "bootstrap";

interface Puesto {
  idPuesto: string;
  Nombre: string;
  Descripcion: string;
  Salario: number;
  idDepartamento: string;
}

interface Departamento {
  idDepartamento: string;
  Nombre: string;
}

export default defineComponent({
  name: "Puesto",
  components: {
    AddPuesto,
    editPuesto,
  },
  setup() {
    const Puestos = ref<Puesto[]>([]);
    const Departamentos = ref<Departamento[]>([]);
    const idPuesto = ref()

    onMounted(async () => {
      await getPuestos();
    });

    const getPuestos = async () => {
      await apiApp.get("/puestos").then((res) => {
        Puestos.value = res.data;
      });
      await apiApp.get("/departamentos").then((res) => {
        Departamentos.value = res.data;
      });
    };

    const getNameDepartamento = (idDep: string) => {
      const dep = Departamentos.value.find(
        (dep) => dep.idDepartamento === idDep
      );
      return dep?.Nombre;
    };

    const EditarPuesto = (id: string) => {  
      idPuesto.value = id
      console.log(idPuesto.value)
      const modal = new Modal(document.getElementById("kt_edit_modal") as Element);
      modal.show();
    };

    const EliminarPuesto = async (idPuesto: string) => {
      apiApp.delete(`/puestos/${idPuesto}`).then(() => {
        getPuestos();
      });
      await getPuestos();
    };

    return {
      getNameDepartamento,
      getPuestos,
      EditarPuesto,
      EliminarPuesto,
      Puestos,
      idPuesto,
    };
  },
});
</script>
