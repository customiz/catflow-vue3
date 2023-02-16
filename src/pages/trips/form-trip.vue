<script>
import CustomerTrip from '@/pages/trips/form-customer.vue'


export default {
  // eslint-disable-next-line vue/component-api-style
  components: {
    CustomerTrip,
  },
  data() {
    return {
      trip_name: "",
      start_date: "",
      end_date: "",
      outbound_flight:"",
      inbound_flight:"",
      guide_name: "",
      hotel:"",
    }
  },
  // eslint-disable-next-line vue/component-api-style
  methods: {
    async createTrip() {
      try {
        const response = await fetch(
          "http://strapiapi.catflows.com/api/trips",
          {
            method: "POST",

            headers: {
              Accept: "application/json",
              "Content-Type": "application/json",

              // Authorization: Bearer ${localStorage.getItem("jwt")},
            },
            body: JSON.stringify({
              data:{
                trip_name: this.trip_name,
                start_date:this.start_date,
                end_date:this.end_date,
                guide_name:this.guide_name,
                outbound_flight:this.outbound_flight,
                inbound_flight:this.inbound_flight,
                hotel:this.hotel,
              },
            
            }),
          },
        ).then(response => response.json())
          .then(data => {
            console.info("trip data : ", data)
            console.debug(data.id)

            const id = data.data.id

            this.$router.push({ name: 'CustomerTrip', params: { id } })

          })

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
          <VForm @submit.prevent="createTrip">
            <VRow class="pa-5">
              <VCol cols="12">
                <h2 class="text-lg font-medium mb-5 text-center">
                  ข้อมูลทริป
                </h2>
                <VRow>
                  <VCol cols="12">
                    <VTextField
                      v-model="trip_name"
                      label="โปรแกรม"
                      required
                    />
                  </VCol>
                </VRow>
                <VRow>
                  <VCol cols="6">
                    <AppDateTimePicker
                      v-model="start_date"
                      density="compact"
                      label="วันเริ่มทริป"
                      style="width: 100%;"
                      :config="{ position: 'auto right' }"
                      required
                    />
                  </VCol>
                  <VCol cols="6">
                    <AppDateTimePicker
                      v-model="end_date"
                      density="compact"
                      label="วันจบทริป"
                      style="width: 100%;"
                      :config="{ position: 'auto right' }"
                      required
                    />
                  </VCol>
                </VRow>
                <VRow>
                  <VCol cols="6">
                    <VTextField
                      v-model="outbound_flight"
                      label="เที่ยวบินขาไป"
                      required
                    />
                  </VCol>  <VCol cols="6">
                    <VTextField
                      v-model="inbound_flight"
                      label="เที่ยวบินขากลับ"
                      required
                    />
                  </VCol>
                </VRow>
                <VRow>
                  <VCol cols="12">
                    <VTextarea
                      v-model="guide_name"
                      label="ไกด์"
                      rows="3"
                    />
                  </VCol>
                </VRow>
                <VRow>
                  <VCol cols="12">
                    <VTextarea
                      v-model="hotel"
                      label="โรงแรม"
                      rows="3"
                    />
                  </VCol>
                </VRow>
              </VCol>
            </VRow>
            <VRow
              class=" mb-3 text-center"
              justify="center"
            >
              <VCol>
                <VBtn
                  :disabled="!trip_name || !start_date || !end_date"
                  type="submit"
                  class=" text-center"
                  color="primary"
                >
                  ถัดไป
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
</style>
