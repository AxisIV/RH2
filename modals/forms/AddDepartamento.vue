<template>
    <!--begin::Modal - New Card-->
    <div
      class="modal fade"
      ref="newCardModalRef"
      id="kt_add_depto_modal"
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
            <h2>Agregar Nuevo Departamento</h2>
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
                  <span class="required">Nombre</span>
                </label>
                <!--end::Label-->
  
                <Field
                  type="text"
                  class="form-control form-control-solid"
                  placeholder=""
                  name="Nombre"
                  v-model="cardData.Nombre"
                />
                <div class="fv-plugins-message-container">
                  <div class="fv-help-block">
                    <ErrorMessage name="Nombre" />
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
                </div>
              </div>
                <!--end::Col-->
                <!--end::Col-->

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
                  <span class="indicator-label"> Submit </span>
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
  
  interface Departamento {
    Nombre: string;
    Descripcion: string;
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
    },
    setup(props) {
      const submitButtonRef = ref<null | HTMLButtonElement>(null);
      const newCardModalRef = ref<null | HTMLElement>(null);
      const Departamentos = ref<Departamento[]>([]);
  
      const cardData = ref<Departamento>({
        Nombre: "",
        Descripcion: "",
      });
  
      const validationSchema = Yup.object().shape({
        Nombre: Yup.string().required().label("Nombre"),
        Descripcion: Yup.string().required().label("Descripcion"),
      });
  
      onMounted(async () => {
        await apiApp.get("/departamentos").then((res) => {
          Departamentos.value = res.data;
        });
      });
  
      const submit = () => {
        if (!submitButtonRef.value) {
          return;
        }
  
        const values = {
          Nombre: cardData.value.Nombre,
          Descripcion: cardData.value.Descripcion,
        };
  
        apiApp.post("/departamentos", values).then((res) => {
          console.log(res);
          Swal.fire({
            text: "Se ha creado el departamento exitosamente",
            icon: "success",
            buttonsStyling: false,
            confirmButtonText: "Okey",
            heightAuto: false,
            customClass: {
              confirmButton: "btn btn-primary",
            },
          }).then(() => {
            props.refresh();
            hideModal(newCardModalRef.value);
          });
        });
      };
  
      return {
        cardData,
        validationSchema,
        submit,
        submitButtonRef,
        newCardModalRef,
        Departamentos,
        getAssetPath,
      };
    },
  });
  </script>
  @/core/helpers/modal
  