<template>
  <div>
    <CRow>
      <CCol :md="12">
        <CCard class="mb-4">
          <CCardHeader> Traffic &amp; Sales </CCardHeader>
          <CCardBody>
            <CTable align="middle" class="mb-0 border" hover responsive>
              <CTableHead color="light">
                <CTableRow>
                  <CTableHeaderCell class="text-center">
                    <CIcon name="cil-people" />
                  </CTableHeaderCell>
                  <CTableHeaderCell>User</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Country</CTableHeaderCell>
                  <CTableHeaderCell>Usage</CTableHeaderCell>
                  <CTableHeaderCell class="text-center">Payment Method</CTableHeaderCell>
                  <CTableHeaderCell>Activity</CTableHeaderCell>
                  <CTableHeaderCell>Action</CTableHeaderCell>
                </CTableRow>
              </CTableHead>
              <CTableBody>
                <CTableRow v-for="item in tableExample" :key="item.name">
                  <CTableDataCell class="text-center">
                    <CAvatar
                      size="md"
                      :src="item.avatar.src"
                      :status="item.avatar.status"
                    />
                  </CTableDataCell>
                  <CTableDataCell>
                    <div>{{ item.user.name }}</div>
                    <div class="small text-medium-emphasis">
                      <span>{{ item.user.new ? "New" : "Recurring" }}</span> |
                      {{ item.user.registered }}
                    </div>
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    <CIcon
                      size="xl"
                      :name="item.country.flag"
                      :title="item.country.name"
                    />
                  </CTableDataCell>
                  <CTableDataCell>
                    <div class="clearfix">
                      <div class="float-start">
                        <strong>{{ item.usage.value }}%</strong>
                      </div>
                      <div class="float-end">
                        <small class="text-medium-emphasis">
                          {{ item.usage.period }}
                        </small>
                      </div>
                    </div>
                    <CProgress thin :color="item.usage.color" :value="item.usage.value" />
                  </CTableDataCell>
                  <CTableDataCell class="text-center">
                    <CIcon size="xl" :name="item.payment.icon" />
                  </CTableDataCell>
                  <CTableDataCell>
                    <div class="small text-medium-emphasis">Last login</div>
                    <strong>{{ item.activity }}</strong>
                  </CTableDataCell>
                  <CTableDataCell>
                    <CButton
                      color="primary"
                      shape="rounded-pill"
                      @click="ShowDetail(item.id)"
                      >View</CButton
                    >
                  </CTableDataCell>
                </CTableRow>
                <CTableRow> </CTableRow>
              </CTableBody>
            </CTable>
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>
    <CModal
      :visible="visibleLiveDemo"
      @close="
        () => {
          visibleLiveDemo = false;
        }
      "
    >
      <CModalHeader>
        <CModalTitle>{{ dataDetail.user.name }}</CModalTitle>
      </CModalHeader>
      <CModalBody>
        <div class="mb-3">
          <div class="float-start">
            <CAvatar :src="dataDetail.avatar.src" /> {{ dataDetail.user.name }}
          </div>
          <div class="float-end">
            <div class="small text-medium-emphasis">Last login</div>
            <strong>{{ dataDetail.activity }}</strong>
          </div>
        </div>
        <div class="mb-3 row pt-4" style="clear: both">
          <label class="form-label col-sm-4">Register</label>
          <label class="form-label col-sm-8">: {{ dataDetail.user.registered }}</label>
        </div>
        <div class="mb-3 row">
          <label class="form-label col-sm-4">Status</label>
          <label class="form-label col-sm-8"
            >: {{ dataDetail.user.new ? "New Member" : "Old Member" }}</label
          >
        </div>
        <div class="mb-3 row">
          <label class="form-label col-sm-4">Period</label>
          <label class="form-label col-sm-8">: {{ dataDetail.usage.period }}</label>
        </div>
        <div class="mb-3 row">
          <label class="form-label col-sm-4">Payment</label>
          <label class="form-label col-sm-8"
            >: <CIcon size="xl" :name="dataDetail.payment.icon"
          /></label>
        </div>
        <div class="mb-3 row">
          <label class="form-label col-sm-4">Country</label>
          <label class="form-label col-sm-8"
            >:
            <CIcon
              size="xl"
              :name="dataDetail.country.flag"
              :title="dataDetail.country.name"
          /></label>
          <div class="mb-3 row">
            <label class="form-label col-sm-4">Usage</label>
            <label class="form-label col-sm-8"
              ><div class="float-start">
                : <strong>{{ dataDetail.usage.value }}%</strong>
              </div>
              <div class="float-end">
                <small class="text-medium-emphasis">
                  {{ dataDetail.usage.period }}
                </small>
              </div>
            </label>
          </div>
        </div>
      </CModalBody>
      <CModalFooter>
        <CButton
          color="secondary"
          @click="
            () => {
              visibleLiveDemo = false;
            }
          "
        >
          Close
        </CButton>
      </CModalFooter>
    </CModal>
  </div>
</template>

<script>
import { CButton } from "@coreui/vue";
import { CModal } from "@coreui/vue";
import dataJson from "../backend/data.json";
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

export default {
  name: "Dashboard",
  components: {},
  setup() {
    const router = useRouter();
    const tableExample = dataJson;
    const dataDetail = ref(null);
    const visibleLiveDemo = ref(false);
    const useraccess = localStorage.getItem("user");

    onMounted(() => {
      if (useraccess === null) {
        router.push("/pages/login");
      }
      console.log(useraccess);
    });
    async function ShowDetail(id) {
      dataDetail.value = null;
      const vdetail = await tableExample.filter((data) => {
        if (data.id === id) {
          return data;
        }
      });
      if (vdetail) {
        dataDetail.value = vdetail[0];
        visibleLiveDemo.value = true;
      }
    }
    return {
      tableExample,
      dataDetail,
      ShowDetail,
      visibleLiveDemo,
      useraccess,
    };
  },
};
</script>
