<script setup>
const printInvoice = () => {
  window.print();
};
</script>

<template>
  <section>
    <VRow>
      <VCol cols="12" md="9">
        <VCard
          class="d-flex d-print-flex flex-column flex-sm-row flex-wrap flex-column flex-sm-row"
        >
          <!-- SECTION Header -->
          <div class="w-100">
            <VCardText
              class="pb-0 d-flex flex-wrap justify-space-between flex-column flex-sm-row flex-column flex-sm-row print-row"
            >
              <div class="ma-sm-4 text-body-2">
                <img
                  src="@images/logosridara.png"
                  width="300"
                  alt=""
                  srcset=""
                />
                <!-- 👉 Address -->
                <p class="mb-0">บริษัท ศรีดาราทัวร์จำากัด (สำนักงานใหญ่)</p>
                <p class="mb-0">
                  123 ถ.ชยางกูร ม.19 ต.บุ่ง อ.เมือง จ.อำานาจเจริญ 37000
                </p>
                <p class="mb-0">เลขประจำตัวผู้เสียภาษี0375552000037</p>
                <p class="mb-0">เบอร์มือถือ 0823656514</p>
                <p class="mb-3">www.facebook.com/sridaratourfanpage</p>
                <p class="mb-1 text-primary">ลูกค้า</p>
                <p class="mb-1">นายณัฐพงศ์ ตั้งสกุลกุลพัฒน์(สำานักงานใหญ่)</p>
                <p class="mb-1">เทศบาลเมืองอำานาเจริญ 37000</p>
                <p class="mb-1">เลขประจำตัวผู้เสียภาษี 3341601163209</p>
              </div>
              <!-- 👉 Content -->
              <div class="mt-4 ma-sm-4">
                <!-- 👉 Invoice ID -->
                <h6 class="font-weight-medium text-xl mb-0 text-primary">
                  ใบกำกับภาษี/ใบเสร็จรับเงิน
                </h6>
                <!-- 👉 Issue Date -->
                <p class="mb-2 text-center text-primary">
                  ต้นฉบับ(เอกสารออกเป็นชุด)
                </p>
                <VDivider />
                <!-- 👉Start Table -->
                <VTable>
                  <thead>
                    <tr>
                      <td scope="col" class="py-3 px-0">
                        <p class="mb-1 text-primary">เลขที่</p>
                        <p class="mb-1 text-primary">วันที่</p>
                        <p class="mb-0 text-primary">ผู้ขาย</p>
                      </td>
                      <td scope="col">
                        <p class="mb-1">INV2022090008</p>
                        <p class="mb-1">09/09/2022</p>
                        <p class="mb-0">SRIDARA PRAPRIAW</p>
                      </td>
                    </tr>
                  </thead>
                </VTable>
                <VDivider />
                <VTable>
                  <thead>
                    <tr>
                      <td scope="col" class="py-3 px-0">
                        <p class="mb-1 text-primary">ชื่องาน</p>
                        <p class="mb-1 text-primary">ผู้ติดต่อ</p>
                        <p class="mb-0 text-primary">เบอร์โทร</p>
                      </td>
                      <td scope="col" class="px-0">
                        <p class="mb-1">ตั๋วเครื่องบิน</p>
                        <p class="mb-1">ณัฐพงศ์</p>
                        <p class="mb-0">-</p>
                      </td>
                    </tr>
                  </thead>
                </VTable>
              </div>
            </VCardText>
            <!-- 👉End Table -->
            <VDivider class="mx-sm-4" />
            <!-- 👉Start Table -->
            <VTable class="mx-sm-4">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">รายละเอียด</th>
                  <th scope="col" class="text-center">จำนวน</th>
                  <th scope="col" class="text-center">ราคาต่อหน่วย</th>
                  <th scope="col" class="text-center">ส่วนลด</th>
                  <th scope="col" class="text-center">ภาษี</th>
                  <th scope="col" class="text-center">มูลค่า</th>
                </tr>
              </thead>
              <tbody class="my-2">
                <tr v-for="(item, index) in items" key="index" class="ma-3">
                  <td>{{ index + 1 }}</td>
                  <td>
                    <v-text-field
                      v-model="item.detail"
                      label="รายละเอียด"
                      variant="outlined"
                    ></v-text-field>
                  </td>
                  <td>
                    <v-text-field
                      v-model="item.count"
                      label="จำนวน"
                      variant="outlined"
                    ></v-text-field>
                  </td>
                  <td>
                    <v-text-field
                      v-model="item.unit"
                      label="ราคาต่อหน่วย"
                      variant="outlined"
                    ></v-text-field>
                  </td>
                  <td>
                    <v-text-field
                      v-model="item.discount"
                      label="ส่วนลด"
                      variant="outlined"
                    ></v-text-field>
                  </td>
                  <td>
                    <v-text-field
                      v-model="item.tax"
                      label="ภาษี"
                      variant="outlined"
                    ></v-text-field>
                  </td>
                  <td>
                    <v-text-field
                      v-model="item.cost"
                      label="มูลค่า"
                      variant="outlined"
                    ></v-text-field>
                  </td>
                  <td>
                    <VBtn
                      icon
                      size="x-small"
                      color="default"
                      variant="text"
                      @click="removeProduct(index)"
                    >
                      <VIcon size="20" icon="tabler-x" />
                    </VBtn>
                  </td>
                </tr>
              </tbody>
            </VTable>
            <!-- 👉End Table -->
            <!-- Total -->
            <div class="mt-4 ma-sm-4">
              <VBtn @click="addItem"> Add Item </VBtn>
            </div>
            <VCardText
              class="d-flex justify-space-between flex-column flex-sm-row print-row align-end text-body-2"
            >
              <div class="my-2 mx-sm-4">(สองพันสองร้อยบาทถ้วน)</div>
              <div class="mx-sm-4">
                <table>
                  <tr>
                    <td class="text-end text-blue">
                      <div class="me-5">
                        <p class="mb-6">รวมเป็นเงิน</p>
                        <p class="mb-2">มูลค่าที่ไม่มี/ยกเว้นภาษี</p>
                        <p class="mb-2">มูลค่าที่คำนวณภาษี</p>
                        <p class="mb-2">ภาษีมูลค่าเพิ่ม 7%</p>
                        <p class="mb-2">จำนวนเงินรวมทั้งสิ้น</p>
                      </div>
                    </td>
                    <td class="font-weight-semibold">
                      <p class="mb-4">2,200.00 บาท</p>
                      <p class="mb-2">100.00 บาท</p>
                      <p class="mb-2">1,962.62 บาท</p>
                      <p class="mb-2">137.38 บาท</p>
                      <p class="mb-2">2,200.00 บาท</p>
                    </td>
                  </tr>
                </table>
              </div>
            </VCardText>
          </div>
          <div class="w-100">
            <VCardText>
              <div class="d-flex mx-sm-4 align-center text-body-2">
                <div>
                  การชำระเงินจะสมบูรณ์เมื่อบริษัทได้รับเงินเรียบร้อยแล้ว
                </div>
                <div class="d-flex align-center mx-2">
                  <input
                    id="cash"
                    type="checkbox"
                    name="type_pay"
                    class="mx-2"
                  />
                  <label for="cash">เงินสด</label>
                </div>
                <div class="d-flex align-center mx-2">
                  <input
                    id="check_money"
                    type="checkbox"
                    name="type_pay"
                    class="mx-2"
                  />
                  <label for="check_money">เช็ค</label>
                </div>
                <div class="d-flex align-center mx-2">
                  <input
                    id="transfer_money"
                    type="checkbox"
                    name="type_pay"
                    class="mx-2"
                  />
                  <label for="transfer_money">โอนเงิน</label>
                </div>
                <div class="d-flex align-center mx-2">
                  <input
                    id="credit_card"
                    type="checkbox"
                    name="type_pay"
                    class="mx-2"
                  />
                  <label for="credit_card">บัตรเครดิต</label>
                </div>
              </div>
            </VCardText>
            <VCardText>
              <div class="py-0 text-body-2 mx-sm-4">
                <VRow>
                  <VCol
                    cols="3"
                    class="d-inline-flex pr-0 align-self-end text-no-wrap"
                  >
                    <div>ธนาคาร</div>
                    <div class="south_line w-100" />
                  </VCol>
                  <VCol
                    cols="3"
                    class="d-flex px-0 align-self-end text-no-wrap"
                  >
                    <div>เลขที่</div>
                    <div class="south_line w-100" />
                  </VCol>
                  <VCol
                    cols="3"
                    class="d-flex px-0 align-self-end text-no-wrap"
                  >
                    <div>วันที่</div>
                    <div class="south_line w-100" />
                  </VCol>
                  <VCol
                    cols="3"
                    class="d-flex pl-0 align-self-end text-no-wrap"
                  >
                    <div>จำนวนเงิน</div>
                    <div class="south_line w-100" />
                  </VCol>
                </VRow>
              </div>
            </VCardText>
            <VCardText class="d-flex w-100">
              <div
                class="d-flex mx-sm-4 align-center text-body-2 justify-space-between w-100"
              >
                <p>ในนาม นายณัฐพงศ์ ตัง้สกุลกุลพัฒน</p>
                <p>ในนาม บริษัท ศรีดาราทัวร์จำากัด</p>
              </div>
            </VCardText>
            <VCardText class="d-flex w-100 mt-5">
              <div
                class="d-flex mx-sm-4 align-center text-body-2 justify-space-between w-100"
              >
                <div class="d-flex">
                  <div class="text-center mr-2 sign">
                    <p>ผู้จ่ายเงิน</p>
                  </div>
                  <div class="text-center sign">
                    <p>วันที่</p>
                  </div>
                </div>
                <div class="d-flex">
                  <div class="text-center mr-2 sign">
                    <p>ผู้รับเงิน</p>
                  </div>
                  <div class="text-center sign">
                    <p>วันที่</p>
                  </div>
                </div>
              </div>
            </VCardText>
          </div>
        </VCard>
      </VCol>

      <VCol cols="12" md="3" class="d-print-none">
        <VCard>
          <VCardText>
            <!-- 👉 Send Invoice Trigger button -->
            <VBtn
              block
              variant="tonal"
              color="secondary"
              class="mb-2"
              @click="printInvoice"
            >
              Print
            </VBtn>
          </VCardText>
        </VCard>
      </VCol>
    </VRow>

    <!-- 👉 Add Payment Sidebar -->
  </section>
</template>

<script>
export default {
  // eslint-disable-next-line vue/component-api-style
  data: () => ({
    items: [
      {
        detail: "test",
        count: 0,
        unit: 0,
        discount: 0,
        tax: 0,
        cost: 0,
      },
    ],
  }),
  // eslint-disable-next-line vue/component-api-style
  methods: {
    addItem() {
      // eslint-disable-next-line vue/no-mutating-props
      this.items.push({
        detail: "test",
        count: 0,
        unit: 0,
        discount: 0,
        tax: 0,
        cost: 0,
      });
      console.log(this.items);
    },
    removeProduct(id) {
      this.items.splice(id, 1)
      console.log(id)
      console.log(this.items)
    },
  },
};
</script>

<style lang="scss">
@media print {
  .v-application {
    background: none !important;
  }

  @page {
    margin: 0;
    size: auto;
  }

  .v-navigation-drawer,
  .layout-vertical-nav,
  .app-customizer-toggler,
  .layout-footer,
  .layout-navbar,
  .layout-navbar-and-nav-container {
    display: none;
  }

  .v-card {
    box-shadow: none !important;

    .d-print-flex {
      display: flex;
      align-content: space-between !important;
      height: 100% !important;
    }

    .print-row {
      flex-direction: row !important;
    }
  }

  .layout-content-wrapper {
    padding-inline-start: 0 !important;
  }
}

.sign {
  border-top: 1px solid #33303cad;
  width: 125px;
}

.south_line {
  border-bottom: 1px solid #33303cad;
  margin-bottom: 5px;
}
</style>
