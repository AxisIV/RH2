<template>
  <!--begin::Modal - New Card-->
  <div
    class="modal fade"
    ref="addPuestoRef"
    id="kt_edit_modal"
    tabindex="-1"
    aria-hidden="true"
  >
    <!--begin::Modal dialog-->
    <div class="modal-dialog modal-dialog-centered mw-650px">
      <!--begin::Modal content-->
      <div class="modal-content">
        <!--begin::Modal header-->
        <div class="modal-header">
          <!--begin::Modal title-->
          <h2>Edit Nuevo Puesto</h2>
          <!--end::Modal title-->

          <!--begin::Close-->
          <div
            class="btn btn-sm btn-icon btn-active-color-primary"
            data-bs-dismiss="modal"
          >
            <KTIcon icon-name="cross" icon-class="fs-1" />
          </div>
          <!--end::Close-->
        </div>
        <!--end::Modal header-->
        
        <!--begin::Modal body-->
        <div class="modal-body scroll-y mx-5 mx-xl-15 my-7">
          <!--begin::Form-->
          <VForm
            id="kt_modal_new_card_form"
            class="form"
            @submit="submit"
            :validation-schema="validationSchema"
          >
            <!--begin::Input group-->
            <div class="d-flex flex-column mb-7 fv-row">
              <!--begin::Label-->
              <label
                class="d-flex align-items-center fs-6 fw-semibold form-label mb-2"
              >
                <span class="required">Puesto</span>
              </label>
              <!--end::Label-->

              <Field
                type="text"
                class="form-control form-control-solid"
                placeholder=""
                name="Puesto"
                v-model="cardData.Puesto"
              />
              <div class="fv-plugins-message-container">
                <div class="fv-help-block">
                  <ErrorMessage name="Puesto" />
                </div>
              </div>
            </div>
            <!--end::Input group-->

            <!--begin::Input group-->
            <div class="d-flex flex-column mb-7 fv-row">
              <!--begin::Label-->
              <label class="required fs-6 fw-semibold form-label mb-2"
                >Descripcion</label
              >
              <!--end::Label-->

              <!--begin::Input wrapper-->
              <div class="position-relative">
                <!--begin::Input-->
                <Field
                  type="text"
                  class="form-control form-control-solid"
                  placeholder=""
                  name="Descripcion"
                  v-model="cardData.Descripcion"
                />
                <div class="fv-plugins-message-container">
                  <div class="fv-help-block">
                    <ErrorMessage name="Descripcion" />
                  </div>
                </div>
                <!--end::Input-->
              </div>
              <!--end::Input wrapper-->
            </div>
            <!--end::Input group-->

            <!--begin::Input group-->
            <!--begin::Row-->
            <div class="row g-9 mb-8">
              <!--begin::Col-->
              <div class="col-md-6 fv-row">
                <label class="required fs-6 fw-semibold form-label mb-2"
                  >Departamento</label
                >
                <Field
                  v-model="cardData.Departamento"
                  name="Departamento"
                  class="form-select form-select-solid"
                  data-control="select2"
                  data-hide-search="true"
                  data-placeholder="Year"
                  as="select"
                >
                  <option></option>
                  <template v-for="(dep, i) in Departamentos" :key="i">
                    <option :value="dep.idDepartamento">
                      {{ dep.Nombre }}
                    </option>
                  </template>
                </Field>
                <div class="fv-plugins-message-container">
                  <div class="fv-help-block">
                    <ErrorMessage name="Departamento" />
                  </div>
                </div>
              </div>
              <!--begin::Col-->
              <div class="col-md-6 fv-row">
                <!--begin::Label-->
                <label class="required fs-6 fw-semibold form-label mb-2"
                  >Salario</label
                >
                <!--end::Label-->
                <div class="col-6">
                  <Field
                    type="text"
                    class="form-control form-control-solid"
                    placeholder=""
                    name="Salario"
                    v-model="cardData.Salario"
                  >
                  </Field>
                  <div class="fv-plugins-message-container">
                    <div class="fv-help-block">
                      <ErrorMessage name="Salario" />
                    </div>
                  </div>
                </div>
              </div>
              <!--end::Col-->
              <!--end::Col-->
            </div>
            <!--end::Input group-->

            <!--begin::Actions-->
            <div class="text-center pt-15">
              <button
                type="reset"
                id="kt_modal_new_card_cancel"
                class="btn btn-light me-3"
              >
                Discard
              </button>

              <button
                ref="submitButtonRef"
                type="submit"
                id="kt_modal_new_card_submit"
                class="btn btn-primary"
              >
                <span class="indicator-label"> Editar </span>
                <span class="indicator-progress">
                  Please wait...
                  <span
                    class="spinner-border spinner-border-sm align-middle ms-2"
                  ></span>
                </span>
              </button>
            </div>
            <!--end::Actions-->
          </VForm>
          <!--end::Form-->
        </div>
        <!--end::Modal body-->
      </div>
      <!--end::Modal content-->
    </div>
    <!--end::Modal dialog-->
  </div>
  <!--end::Modal - New Card-->
</template>

<script lang="ts">
import { getAssetPath } from "@/core/helpers/assets";
import { defineComponent, ref, onMounted } from "vue";
import { ErrorMessage, Field, Form as VForm } from "vee-validate";
import Swal from "sweetalert2/dist/sweetalert2.js";
import { hideModal } from "@/core/helpers/modal";
import * as Yup from "yup";
import { apiApp } from "@/core/api/apiApp";

interface Puesto {
  Puesto: string;
  Descripcion: string;
  Departamento: string;
  Salario: string;
}

interface Departamento {
  idDepartamento: string;
  Nombre: string;
}

export default defineComponent({
  name: "new-card-modal",
  components: {
    ErrorMessage,
    Field,
    VForm,
  },
  props: {
    refresh: {
      type: Function,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const submitButtonRef = ref<null | HTMLButtonElement>(null);
    const addPuestoRef = ref<null | HTMLElement>(null);
    const Departamentos = ref<Departamento[]>([]);
    const puesto = ref()

    const cardData = ref<Puesto>({
      Puesto: "",
      Descripcion: "",
      Departamento: "",
      Salario: "",
    });

    const validationSchema = Yup.object().shape({
      Puesto: Yup.string().required().label("Puesto"),
      Descripcion: Yup.string().required().label("Descripcion"),
      Departamento: Yup.string().required().label("Departamento"),
      Salario: Yup.string().required().label("Salario"),
    });

    onMounted(async () => {
      await apiApp.get("/puestos/"+ props.id).then((res) => {
        puesto.value = res.data
        cardData.value = {
          Puesto: puesto.value.Nombre,
          Descripcion: puesto.value.Descripcion,
          Departamento: puesto.value.idDepartamento,
          Salario: puesto.value.Salario,
        }

      })
      await apiApp.get("/departamentos").then((res) => {
        Departamentos.value = res.data;
      });
    });

    const submit = () => {
      if (!submitButtonRef.value) {
        return;
      }

      const values = {
        Nombre: cardData.value.Puesto,
        Descripcion: cardData.value.Descripcion,
        idDepartamento: cardData.value.Departamento,
        Salario: cardData.value.Salario,
      };

      apiApp.put("/puestos/"+ props.id, values).then((res) => {
        console.log(res);
        Swal.fire({
          text: "Se ha creado el puesto exitosamente",
          icon: "success",
          buttonsStyling: false,
          confirmButtonText: "Okey",
          heightAuto: false,
          customClass: {
            confirmButton: "btn btn-primary",
          },
        }).then(() => {
          props.refresh();
          hideModal(addPuestoRef.value);
        });
      });
    };

    return {
      cardData,
      validationSchema,
      submit,
      submitButtonRef,
      addPuestoRef,
      Departamentos,
      getAssetPath,
    };
  },
});
</script>
@/core/helpers/modal
