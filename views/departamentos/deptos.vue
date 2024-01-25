<template>
  <div>
    <div class="card shadow-sm p-6">
      <div class="card-header">
        <div>
          <button
            class="btn btn-light-primary gap-2"
            data-bs-toggle="modal"
            data-bs-target="#kt_add_depto_modal"
          > 
            <i class="bi bi-plus fs-1"
              ><span class="path1"></span><span class="path2"></span
              ><span class="path3"></span><span class="path4"></span
            ></i>
            Agregar Departamento
          </button>
        </div>
      </div>
      <div class="py-5">
        <div class="table-responsive">
          <table class="table table-row-dashed table-row-gray-300 gy-7">
            <thead>
              <tr class="fw-bold fs-6 text-gray-800">
                <th>Nombre</th>
                <th>Descripcion</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(departamento, index) in departamentos">
                <td>{{ departamento.Nombre }}</td>
                <td class="mw-100px">{{ departamento.Descripcion }}</td>
                <td>
                  <div class="d-flex flex-shrink-0">
                    <a
                      class="btn btn-icon btn-bg-light btn-active-color-primary btn-sm me-1"
                      @click="Editardepartamento(departamento.idDepartamento)"
                    >
                      <i class="bi bi-pencil-fill fs-3"></i>
                    </a>
                    <a
                      @click="Eliminardepartamento(departamento.idDepartamento)"
                      class="btn btn-icon btn-bg-light btn-active-color-danger btn-sm"
                      data-bs-toggle="modal"
                      data-bs-target="#kt_delete_departamento_modal"
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
  <AddDepartamento :refresh="getdepartamentos" />
  <EditDepto v-if="idDepto"  :refresh="getdepartamentos" :id="idDepto"/>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import { apiApp } from "@/core/api/apiApp";
import AddDepartamento from "@/components/modals/forms/AddDepartamento.vue";
import EditDepto from "@/components/modals/forms/EditDepto.vue";
import { Modal } from "bootstrap";

interface departamento {
  iddepartamento: string;
  Nombre: string;
  Descripcion: string;
  idDepartamento: string;
}

interface Departamento {
  idDepartamento: string;
  Nombre: string;
}

export default defineComponent({
  name: "departamento",
  components: {
    AddDepartamento,
    EditDepto,
  },
  setup() {
    const departamentos = ref<departamento[]>([]);
    const Departamentos = ref<Departamento[]>([]);
    const idDepto = ref()

    onMounted(async () => {
      await getdepartamentos();
    });

    const getdepartamentos = async () => {
      await apiApp.get("/departamentos").then((res) => {
        departamentos.value = res.data;
      });
    };

    const getNameDepartamento = (idDep: string) => {
      const dep = Departamentos.value.find(
        (dep) => dep.idDepartamento === idDep
      );
      return dep?.Nombre;
    };

    const Editardepartamento = (id: string) => {
      idDepto.value = id
      console.log(idDepto.value)
      const modal = new Modal(document.getElementById("kt_edit_modal") as Element);
      modal.show();
    };

    const Eliminardepartamento = async (iddepartamento: string) => {
      apiApp.delete(`/departamentos/${iddepartamento}`).then(() => {
        getdepartamentos();
      });
      await getdepartamentos();
    };

    return {
      getNameDepartamento,
      getdepartamentos,
      Editardepartamento,
      Eliminardepartamento,
      departamentos,
      idDepto,
    };
  },
});
</script>
