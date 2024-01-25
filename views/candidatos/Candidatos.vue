<template>
  <div>
    <div class="card shadow-sm p-6">
      <div class="card-header">
        <div>
          <button
            class="btn btn-light-primary gap-2"
            data-bs-toggle="modal"
            data-bs-target="#kt_add_candidato_modal"
          >
            <i class="bi bi-plus fs-1"
              ><span class="path1"></span><span class="path2"></span
              ><span class="path3"></span><span class="path4"></span
            ></i>
            Agregar Candidato
          </button>
        </div>
      </div>
      <div class="py-5">
        <div class="table-responsive">
          <table class="table table-row-dashed table-row-gray-300 gy-7">
            <thead>
              <tr class="fw-bold fs-6 text-gray-800">
                <th>Nombre</th>
                <th>Dirección</th>
                <th>Teléfono</th>
                <th>Email</th>
                <th>Estado</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(candidato, index) in candidatos">
                <td>{{ candidato.Nombre }}</td>
                <td class="mw-100px">{{ candidato.Direccion }}</td>
                <td class="mw-100px">{{ candidato.Telefono }}</td>
                <td class="mw-100px">{{ candidato.Email }}</td>
                <td>
                  <div class="d-flex flex-shrink-0">
                    <a
                      class="btn btn-icon btn-bg-light btn-active-color-primary btn-sm me-1"
                      data-bs-toggle="modal"
                      data-bs-target="#kt_edit_candidato_modal"
                      @click="Editarcandidato(candidato.idcandidato)"
                    >
                      <i class="bi bi-pencil-fill fs-3"></i>
                    </a>
                    <a
                      @click="Eliminarcandidato(candidato.idcandidato)"
                      class="btn btn-icon btn-bg-light btn-active-color-danger btn-sm"
                      data-bs-toggle="modal"
                      data-bs-target="#kt_delete_candidato_modal"
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
  <AddCandidatos :refresh="getcandidatos" />
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import { apiApp } from "@/core/api/apiApp";
import AddCandidatos from "@/components/modals/forms/AddCandidatos.vue";

interface candidato {
  Nombre: string;
  Direccion: string;
  Telefono: string;
  Email: string;
  idcandidato: string;
}

interface candidato {
  idcandidato: string;
  Nombre: string;
}

export default defineComponent({
  name: "candidato",
  components: {
    AddCandidatos,
  },
  setup() {
    const candidatos = ref<candidato[]>([]);

    onMounted(async () => {
      await getcandidatos();
    });

    const getcandidatos = async () => {
      await apiApp.get("/candidatos").then((res) => {
        candidatos.value = res.data;
      });
    };

    const getNamecandidato = (idDep: string) => {
      const dep = candidatos.value.find(
        (dep) => dep.idcandidato === idDep
      );
      return dep?.Nombre;
    };

    const Editarcandidato = (idcandidato: string) => {
      console.log(idcandidato);
    };

    const Eliminarcandidato = async (idcandidato: string) => {
      apiApp.delete(`/candidatos/${idcandidato}`).then(() => {
        getcandidatos();
      });
      await getcandidatos();
    };

    return {
      getNamecandidato,
      getcandidatos,
      Editarcandidato,
      Eliminarcandidato,
      candidatos,
    };
  },
});
</script>

