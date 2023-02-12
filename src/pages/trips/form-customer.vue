<script>
export default {
  name: "CustomerTrip",
  // eslint-disable-next-line vue/component-api-style
  data() {
    return {
      // customers
      customers: [
        {
          prefix: "",
          fname: "",
          lname: "",
          en_fname: "",
          en_lname: "",
          birthdate: "",
          birth_location: "",
          passport_name: "",
          passport_start: "",
          passport_end: "",
          room: "",
          note: "",
        },
      ],
      thNameTypes: ["นาย", "นาง", "นางสาว"],
    }
  },
  // eslint-disable-next-line vue/component-api-style
  computed:{
    isFormValid(){
      return this.customers.every(customer => (
        customer.fname &&
        customer.lname &&
        customer.birthdate &&
        passport_start &&
        passport_end

      ))
    },
  },
  // eslint-disable-next-line vue/component-api-style
  mounted() {
    this.fetchData()
  },
  // eslint-disable-next-line vue/component-api-style
  methods: {
    addCustomer() {
      this.customers.push({
        prefix: "",
        fname: "",
        lname: "",
        en_fname: "",
        en_lname: "",
        birthdate: "",
        birth_location: "",
        passport_name: "",
        passport_start: "",
        passport_end: "",
        room: "",
        note: "",
      })
    },
    async createCustomer() {
      for (const element of this.customers) {
        let cus = { 
          data: Object.assign(
            { 
              trip_id: this.idTrip.id,
              prefix: element.prefix || "",
              en_fname: element.en_fname || "",
              en_lname: element.en_lname || "",
              birth_location: element.birth_location || "",
              passport_name: element.passport_name || "",
              room: element.room || "",
              note: element.note || "",
            }, 
            element,
          ), 
        }
        console.log(JSON.stringify(cus))
        try {
          const response = await fetch(
            "http://strapiapi.catflows.com/api/customers",
            {
              method: "POST",
              headers: {
                Accept: "application/json",
                "Content-Type": "application/json",

                // Authorization: Bearer ${localStorage.getItem("jwt")},
              },
              body: JSON.stringify({
                data: {
                  trip_id: this.idTrip.id,
                  prefix: element.prefix,
                  fname: element.fname,
                  lname: element.lname,
                  en_fname: element.en_fname,
                  en_lname: element.en_lname,
                  birthdate: element.birthdate,
                  birth_location: element.birth_location,
                  passport_name: element.passport_name,
                  passport_start: element.passport_start,
                  passport_end: element.passport_end,
                  room: element.room,
                  note: element.note,
                },
              }),
              
            },console.log(this.trip_id),
          )

          const data = await response.json()

          console.log(data)
          console.info("customer data : ", data)
          console.debug(data)
        } catch (error) {
          console.error(error)
        }
      }
    },
    async fetchData() {
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
      }
    },
  },
}
</script>

<template>
  <section>
    <VRow>
      <VCol cols="12">
        <VCard>
          <VRow>
            <VCol>
              <h2 class="text-lg font-medium mt-5 text-center">
                ข้อมูลทริป {{ idTrip.id }}
              </h2>
            </VCol>
          </VRow>
          <VRow class="mx-5">
            <VCol cols="6">
              <p><span class="font-weight-bold">โปรแกรม : </span>{{ trip.attributes.trip_name }}</p>
            </VCol>
            <VCol cols="6">
              <p><span class="font-weight-bold">โรงแรม : </span>{{ latestTrip.hotel }}</p>
            </VCol>
          </VRow>
          <VRow class="mx-5">
            <VCol cols="6">
              <p><span class="font-weight-bold">วันเริ่มทริป : </span>{{ latestTrip.start_date }}</p>
            </VCol>
            <VCol cols="6">
              <p><span class="font-weight-bold">วันจบทริป : </span>{{ latestTrip.end_date }}</p>
            </VCol>
          </VRow>
          <VRow class="mx-5">
            <VCol cols="6">
              <p><span class="font-weight-bold">เที่ยวบินขาไป : </span>{{ latestTrip.outbound_flight }}</p>
            </VCol>
            <VCol cols="6">
              <p><span class="font-weight-bold">เที่ยวบินขากลับ : </span>{{ latestTrip.inbound_flight }}</p>
            </VCol>
          </VRow>
          <VDivider class="mt-6" />
          <VForm @submit.prevent="createCustomer">
            <VRow class="mx-5">
              <VCol cols="12">
                <h2 class="text-lg font-medium my-5 text-center">
                  ข้อมูลลูกค้า
                </h2>
                <VRow
                  v-for="(customer, index) in customers"
                  :key="index"
                  class="pa-3 my-3 customer"
                >
                  <VCol>
                    <VRow>
                      <VCol cols="12">
                        <VSelect
                          v-model="customer.prefix"
                          :items="thNameTypes"
                          label="คำนำหน้าชื่อ"
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="6">
                        <VTextField
                          v-model="customer.fname"
                          label="ชื่อ"
                          required
                        />
                      </VCol>
                      <VCol cols="6">
                        <VTextField
                          v-model="customer.lname"
                          label="นามสกุล"
                          required
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="6">
                        <VTextField
                          v-model="customer.en_fname"
                          label="ชื่อภาษาอังกฤษ"
                        />
                      </VCol>
                      <VCol cols="6">
                        <VTextField
                          v-model="customer.en_lname"
                          label="นามสกุลภาษาอังกฤษ"
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="6">
                        <AppDateTimePicker
                          v-model="customer.birthdate"
                          density="compact"
                          label="วันเดือนปีเกิด"
                          style="width: 100%;"
                          :config="{ position: 'auto right' }"
                          required
                        />
                      </VCol>
                      <VCol cols="6">
                        <VTextField
                          v-model="customer.birth_location"
                          label="สถานที่เกิด"
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="12">
                        <VTextField
                          v-model="customer.passport_name"
                          label="หมายเลขพาสปอร์ต"
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="6">
                        <AppDateTimePicker
                          v-model="customer.passport_start"
                          density="compact"
                          label="วันที่ออกบัตร"
                          style="width: 100%;"
                          :config="{ position: 'auto right' }"
                        />
                      </VCol>
                      <VCol cols="6">
                        <AppDateTimePicker
                          v-model="customer.passport_end"
                          density="compact"
                          label="วันที่บัตรหมดอายุ"
                          style="width: 100%;"
                          :config="{ position: 'auto right' }"
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="12">
                        <VTextField
                          v-model="customer.room"
                          label="ห้อง"
                        />
                      </VCol>
                    </VRow>
                    <VRow>
                      <VCol cols="12">
                        <VTextarea
                          v-model="customer.note"
                          label="หมายเหตุ"
                          rows="3"
                        />
                      </VCol>
                    </VRow>
                  </VCol>
                </VRow>
                <VRow style="margin-left: 0;">
                  <VCol>
                    <VBtn
                      class="text-center"
                      color="primary"
                      @click="addCustomer"
                    >
                      เพิ่มลูกค้า
                    </VBtn>
                  </VCol>
                </VRow>
              </VCol>
            </VRow>
            <VRow
              class="ma-3 text-center"
              justify="center"
            >
              <VCol>
                <VBtn
                  class="text-center"
                  style=" margin-left: 4px;"
                  color="secondary"
                  to="form-trip"
                >
                  กลับ
                </VBtn>
                <VBtn
                  :disabled="!isFormValid"
                  type="submit"
                  class="text-center"
                  style=" margin-left: 4px;"
                  color="primary"
                >
                  ยืนยัน
                </VBtn>
              </VCol>
            </VRow>
          </VForm>
        </VCard>
      </VCol>
    </VRow>
  </section>
</template>

<style scoped>
.customer,
.hotel {
  border: 1px solid rgb(215, 215, 215);
  border-radius: 6px;
}

.p {
  padding: 0;
  margin-block-end: 0;
}
</style>
