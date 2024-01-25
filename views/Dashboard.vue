<template>
  <!--begin::Container-->
  <div class="container-xl" id="kt_content_container">
    <!--begin::Row-->
    <div class="row g-5 g-xl-8">
      <!--begin::Col-->
      <div class="col-3">
        <!--begin::Card-->
        <div class="card card-stretch shadow p-3 mb-5 bg-white rounded">
          <!--begin::Link-->
          <router-link
            :to="{ name: 'Puestos' }"
            class="btn btn-flex btn-text-gray-800 btn-icon-gray-400 btn-active-color-primary bg-body flex-column justfiy-content-start align-items-start text-start w-100 p-10"
          >
            <i class="bi bi-suitcase-lg fs-2tx mb-5 ms-n1">
              <span class="path1"></span>
              <span class="path2"></span>
              <span class="path3"></span>
              <span class="path4"></span>
            </i>
            <span class="fs-4 fw-bold">Puestos</span>
          </router-link>
          <!--end::Link-->
        </div>
        <!--end::Card-->
      </div>
      <!--end::Col-->
      <!--begin::Col-->
      <div class="col-3">
        <!--begin::Card-->
        <div class="card card-stretch shadow p-3 mb-5 bg-white rounded">
          <!--begin::Link-->
          <router-link
            :to="{ name: 'Chats' }"
            class="btn btn-flex btn-text-gray-800 btn-icon-gray-400 btn-active-color-primary bg-body flex-column justfiy-content-start align-items-start text-start w-100 p-10"
          >
            <i class="bi bi-whatsapp fs-2tx mb-5 ms-n1">
              <span class="path1"></span>
              <span class="path2"></span>
              <span class="path3"></span>
              <span class="path4"></span>
            </i>
            <span class="fs-4 fw-bold">Chats</span>
          </router-link>
          <!--end::Link-->
        </div>
        <!--end::Card-->
      </div>
      <!--end::Col-->
      <!--begin::Col-->
      <div class="col-3">
        <!--begin::Card-->
        <div class="card card-stretch shadow p-3 mb-5 bg-white rounded">
          <!--begin::Link-->
          <router-link
            :to="{ name: 'Candidatos' }"
            class="btn btn-flex btn-text-gray-800 btn-icon-gray-400 btn-active-color-primary bg-body flex-column justfiy-content-start align-items-start text-start w-100 p-10"
          >
            <i class="bi bi-gear-wide-connected fs-2tx mb-5 ms-n1">
              <span class="path1"></span>
              <span class="path2"></span>
              <span class="path3"></span>
              <span class="path4"></span>
            </i>
            <span class="fs-4 fw-bold">Candidatos</span>
          </router-link>
          <!--end::Link-->
        </div>
        <!--end::Card-->
      </div>
      <!--end::Col-->
      <!--begin::Col-->
      <div class="col-3">
        <!--begin::Card-->
        <div class="card card-stretch shadow p-3 mb-5 bg-white rounded">
          <!--begin::Link-->
          <router-link
            :to="{ name: 'Departamentos' }"
            class="btn btn-flex btn-text-gray-800 btn-icon-gray-400 btn-active-color-primary bg-body flex-column justfiy-content-start align-items-start text-start w-100 p-10"
          >
            <i class="bi bi-building-add fs-2tx mb-5 ms-n1">
              <span class="path1"></span>
              <span class="path2"></span>
              <span class="path3"></span>
              <span class="path4"></span>
            </i>
            <span class="fs-4 fw-bold">Departamentos</span>
          </router-link>
          <!--end::Link-->
        </div>
        <!--end::Card-->
      </div>
      <!--end::Col-->
    </div>
    <!--end::Row-->
  </div>
  <!--end::Container-->
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "main-dashboard",
  components: {},
});
</script>
