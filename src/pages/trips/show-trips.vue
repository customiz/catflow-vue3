<script>
export default {
  name: "ShowTrips",
  data() {
    return {
      trips: [],
    }
  },
  mounted() {
    this.getTripById()
  },
  methods: {
    async getTripById() {
      try {
        const id = this.$route.params.id
        const response = await fetch(`http://strapiapi.catflows.com/api/trips/${id}`)
        const trip = await response.json()

        this.trips = [trip]
        console.log(trip)
        console.info("trip", trip)
        console.debug(trip)
      } catch (error) {
        console.error(error)
        console.debug(error)
      }
    },
  },
}
</script>


<script setup>
import { useUserListStore } from "@/views/apps/user/useUserListStore";
import logo from "@images/avatars/logosridara.png";

const userListStore = useUserListStore()
const searchQuery = ref("")
const selectedRole = ref()
const selectedPlan = ref()
const selectedStatus = ref()
const rowPerPage = ref(10)
const currentPage = ref(1)
const totalPage = ref(1)
const totalUsers = ref(0)
const users = ref([])

const fetchUsers = () => {
  userListStore
    .fetchUsers({
      q: searchQuery.value,
      status: selectedStatus.value,
      plan: selectedPlan.value,
      role: selectedRole.value,
      perPage: rowPerPage.value,
      currentPage: currentPage.value,
    })
    .then(response => {
      users.value = response.data.users
      totalPage.value = response.data.totalPage
      totalUsers.value = response.data.totalUsers
    })
    .catch(error => {
      console.error(error)
    })
}

watchEffect(fetchUsers)

watchEffect(() => {
  if (currentPage.value > totalPage.value) currentPage.value = totalPage.value
})

const companyOptions = [{
  avatar: logo,
  title: "ศรีดารา ทัวร์",
  subtitle: "123 ถนนชยางกูร ตรอก/ซอย - ตำบล/แขวง บุ่ง อำเภอ/เขต เมืองอำนาจเจริญ จังหวัด อำนาจเจริญ 37000",
  contact: "080-1615100",
} ]
</script>

<template>
  <section>
    <VCard class="px-10 pb-10 pt-5">
      <VCardText class="d-flex flex-wrap justify-space-between flex-column flex-sm-row print-row">
        <div>
          <div
            v-for="card in companyOptions"
            :key="card.name"
          >
            <VImg
              :src="card.avatar"
              :width="200"
            />
          </div>
    
          <h6 class="font-weight-medium text-xl mb-2 mt-3">
            NAMELISTGROUP
          </h6>
    
          <p class="mb-0">
            PROGRAM :
          </p>
    
          <p class="mb-0">
            DAY :
          </p>
    
          <p class="mb-0">
            FILGHT :
          </p>
    
          <h6 class="font-weight-medium text-xl mb-6">
            TOTAL :
          </h6>
        </div>
    
        <div class="mt-4 ma-sm-4">
          <p class="mb-2 mt-15">
            <span>HOTEL : </span>
    
            <span class="font-weight-semibold" />
          </p>
    
          <p class="mb-2">
            <span>CHECK IN : </span>
    
            <span class="font-weight-semibold" />
          </p>
    
          <p class="mb-2">
            <span>CHECK OUT : </span>
    
            <span class="font-weight-semibold" />
          </p>
    
          <h6 class="font-weight-medium text-xl mb-6">
            GUIDE :
          </h6>
        </div>
      </VCardText>
    
    
    
      <VCardText class="d-flex flex-wrap py-4 gap-4">
        <div
          class="d-flex align-center"
          style="width: 135px;"
        >
          <span class="text-no-wrap me-3">แสดง:</span>
    
          <VSelect
            v-model="rowPerPage"
            density="compact"
            :items="[10, 20, 30, 50]"
          />
        </div>
    
    
    
        <VSpacer />
    
        <VBtn
          prepend-icon="tabler-plus"
          @click="isAddNewUserDrawerVisible = true"
        >
          เพิ่มทัวร์
        </VBtn>
    
        <div class="d-flex align-center flex-wrap gap-4">
          <!-- 👉 Search  -->
    
          <div style="width: 15rem;">
            <VTextField
              v-model="searchQuery"
              placeholder="ค้นหา"
              density="compact"
            />
          </div>
        </div>
      </VCardText>
    
    
    
      <VDivider />
    
    
    
      <VTable class="text-no-wrap">
        <thead>
          <tr>
            <th scope="col">
              ลำดับ
            </th>
    
            <th scope="col">
              ชื่อภาษาไทย
            </th>
    
            <th scope="col">
              Name
            </th>
    
            <th scope="col">
              NO.Passport
            </th>
    
            <th scope="col">
              วันที่ออก
            </th>
    
            <th scope="col">
              วันที่หมด
            </th>
    
            <th scope="col">
              ว/ด/ป เกิด
            </th>
    
            <th scope="col">
              ห้อง
            </th>
    
            <th scope="col">
              หมายเหตุ
            </th>
          </tr>
        </thead>
    
    
    
        <tbody>
          <tr style="height: 3.75rem;">
            <td class="text-center" />
    
            <td class="text-capitalize text-base font-weight-semibold" />
    
            <td class="text-center" />
    
            <td class="text-center" />
    
            <td class="text-center" />
    
            <td class="text-center" />
    
            <td class="text-center" />
    
            <td class="text-center" />
    
            <td class="text-center" />
          </tr>
        </tbody>
    
    
    
        <tfoot v-show="!users.length">
          <tr>
            <td
              colspan="7"
              class="text-center"
            >
              No data available
            </td>
          </tr>
        </tfoot>
      </VTable>
    
    
    
      <VDivider />
    
    
    
      <VCardText class="d-flex align-center flex-wrap justify-space-between gap-4 py-3 px-5">
        <span class="text-sm text-disabled">
    
          {{ paginationData }}
    
        </span>
    
    
    
        <VPagination
          v-model="currentPage"
          size="small"
        />
      </VCardText>
    </VCard>
  </section>
</template>

<style lang="scss">
.app-user-search-filter {
  inline-size: 31.6rem;
}

.text-capitalize {
  text-transform: capitalize;
}

.user-list-name:not(:hover) {
  color: rgba(var(--v-theme-on-background), var(--v-high-emphasis-opacity));
}

.v-banner--density-default.v-banner--two-line {
  padding-block: 0 16px;
}

p {
  margin-block-end: 0;
}
</style>



