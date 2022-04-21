<template>
  <v-row>
    <v-col cols="12" sm="8" md="12">
      <v-data-table
        :headers="headers"
        :items="store"
        sort-by="store_name"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>ข้อมูลร้านค้าทั้งหมด</v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  color="success"
                  dark
                  class="mb-2"
                  v-bind="attrs"
                  v-on="on"
                >
                  <v-icon>mdi-archive-plus</v-icon>เพิ่มร้านค้า
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span
                    class="text-h5"
                    style="font-family: 'Prompt', sans-serif"
                    >{{ formTitle }}</span
                  >
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="6" md="7">
                        <v-text-field
                          v-model="formadd.store_name"
                          label="ชื่อร้านค้า"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="5">
                        <v-text-field
                          v-model="formadd.phone_number"
                          label="เบอร์ติดต่อร้านค้า"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="12">
                        <v-text-field
                          v-model="formadd.detail"
                          label="คำอธิบายร้านค้า"
                        ></v-text-field>
                      </v-col>

                      <v-col cols="12" sm="6" md="12">
                        <v-textarea
                          outlined
                          name="input-7-4"
                          label="ที่อยู่"
                          v-model="formadd.address"
                        ></v-textarea>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">
                    ยกเลิก
                  </v-btn>
                  <v-btn color="blue darken-1" text @click="save(formadd.id)">
                    บันทึก
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-dialog
              v-model="dialogDelete"
              fullscreen
              hide-overlay
              transition="dialog-bottom-transition"
            >
              <v-card>
                <v-toolbar dark color="primary">
                  <v-btn icon dark @click="dialogDelete = false">
                    <v-icon>mdi-close</v-icon>
                  </v-btn>
                  <v-toolbar-title>รายละเอียดข้อมูลร้านค้า</v-toolbar-title>
                  <v-spacer></v-spacer>
                </v-toolbar>
                <v-container class="mt-3 mb-3">
                  <v-row>
                    <v-col cols="12" md="6">
                      <h3>ชื่อร้านค้า : {{ detail_Store.store_name }}</h3>
                    </v-col>
                    <v-col cols="12" md="6">
                      <h3>
                        เบอร์ติดต่อร้านค้า : {{ detail_Store.phone_number }}
                      </h3>
                    </v-col>
                    <v-col cols="12" md="6">
                      <h3>คำอธิบายร้านค้า : {{ detail_Store.detail }}</h3>
                    </v-col>
                    <v-col cols="12" md="6">
                      <h3>ที่อยู่ : {{ detail_Store.address }}</h3>
                    </v-col>
                  </v-row>
                </v-container>
                <hr class="style5" />
                <div>
                  <h2 class="ml-5 mt-3">ข้อมูลสินค้าทั้งหมดที่อยู่ในร้านค้า</h2>

                  <v-container>
                    <v-row>
                      <v-col cols="12" md="12">
                        <v-data-table
                          :headers="headers2"
                          :items="products"
                          class="elevation-1"
                        >
                          <template v-slot:top>
                            <v-toolbar flat>
                              <v-toolbar-title
                                >ข้อมูลสินค้าทั้งหมด</v-toolbar-title
                              >
                              <v-divider
                                class="mx-4"
                                inset
                                vertical
                              ></v-divider>
                              <v-spacer></v-spacer>
                              <v-dialog v-model="dialog2" max-width="500px">
                                <template v-slot:activator="{ on, attrs }">
                                  <v-btn
                                    color="success"
                                    dark
                                    class="mb-2"
                                    v-bind="attrs"
                                    v-on="on"
                                  >
                                    <v-icon>mdi-package-variant-plus</v-icon>
                                    เพิ่มสินค้า
                                  </v-btn>
                                </template>
                                <v-card>
                                  <v-card-title>
                                    <span class="text-h5">เพิ่มสินค้า</span>
                                  </v-card-title>

                                  <v-card-text>
                                    <v-container>
                                      <v-row>
                                        <v-col cols="12" sm="6" md="7">
                                          <v-text-field
                                            v-model="productForm.product_name"
                                            label="ชื่อสินค้า"
                                          ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" sm="6" md="5">
                                          <v-text-field
                                            v-model="productForm.price"
                                            type="number"
                                            label="ราคาสินค้า"
                                          ></v-text-field>
                                        </v-col>
                                        <v-col cols="12" sm="6" md="12">
                                          <v-text-field
                                            v-model="productForm.detail"
                                            label="รายละเอียดสินค้า"
                                          ></v-text-field>
                                        </v-col>

                                        <v-col cols="12" sm="6" md="12">
                                          <v-text-field
                                            v-model="productForm.unit"
                                            label="หน่วยสินค้า"
                                          ></v-text-field>
                                        </v-col>
                                        <v-col class="d-flex" cols="12" sm="6">
                                          <v-select
                                            v-model="
                                              productForm.id_product_cate
                                            "
                                            :items="productsCate"
                                            item-text="category_name"
                                            item-value="id"
                                            label="หมวดหมู่สินค้า"
                                          ></v-select>
                                        </v-col>
                                      </v-row>
                                    </v-container>
                                  </v-card-text>

                                  <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn
                                      color="blue darken-1"
                                      text
                                      @click="close"
                                    >
                                      ยกเลิก
                                    </v-btn>
                                    <v-btn
                                      color="blue darken-1"
                                      text
                                      @click="createProduct(detail_Store.id)"
                                    >
                                      บันทึก
                                    </v-btn>
                                  </v-card-actions>
                                </v-card>
                              </v-dialog>
                            </v-toolbar>
                          </template>
                          <template v-slot:item.actions="{ item }">
                            <v-icon
                              small
                              class="mr-2"
                              @click="editProduct(item)"
                              style="font-size: 25px"
                            >
                              mdi-pencil
                            </v-icon>
                            <v-icon
                              small
                              color="red"
                              @click="deleteItem(item)"
                              style="font-size: 25px"
                            >
                              mdi-delete
                            </v-icon>
                          </template>
                        </v-data-table>
                      </v-col>
                    </v-row>
                  </v-container>
                </div>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            style="font-size: 25px"
            @click="editItem(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            style="font-size: 25px"
            @click="
              detailStore(item), getProductInStore(item), getProductCate()
            "
          >
            mdi-information
          </v-icon>
        </template>
      </v-data-table>
    </v-col>

    <v-dialog v-model="dialog3" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">แก้ไขสินค้า</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="7">
                <v-text-field
                  v-model="productForm.product_name"
                  label="ชื่อสินค้า"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="5">
                <v-text-field
                  v-model="productForm.price"
                  label="ราคาสินค้า"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="12">
                <v-text-field
                  v-model="productForm.detail"
                  label="รายละเอียดสินค้า"
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="6" md="12">
                <v-text-field
                  v-model="productForm.unit"
                  label="หน่วยสินค้า"
                ></v-text-field>
              </v-col>
              <v-col class="d-flex" cols="12" sm="6">
                <v-select
                  v-model="productForm.id_product_cate"
                  :items="productsCate"
                  item-text="category_name"
                  item-value="id"
                  label="หมวดหมู่สินค้า"
                ></v-select>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="close"> ยกเลิก </v-btn>
          <v-btn color="blue darken-1" text @click="updateProduct(productForm)">
            บันทึก
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogDelete2" max-width="500px">
      <v-card>
        <v-card-title class="text-h5"
          >ตุณต้องการลบสินค้านี้หรือไม่?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDelete">ยกเลิก</v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="deleteItemConfirm(id, id_store)"
            >ยืนยัน</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      dialog: false,
      dialog2: false,
      dialog3: false,
      dialogDelete: false,
      dialogDelete2: false,
      headers: [
        {
          text: 'ชื่อร้านค้า',
          align: 'start',
          sortable: false,
          value: 'store_name',
        },
        { text: 'คำอธิบายร้านค้า', value: 'detail' },
        { text: 'เบอร์ติดต่อร้านค้า', value: 'phone_number' },
        { text: 'ที่อยู่', value: 'address' },
        { text: 'ตัวเลือก', value: 'actions' },
      ],
      store: [],
      store2: [],
      editedIndex: -1,
      editedIndex2: -1,
      formadd: {
        store_name: '',
        detail: '',
        phone_number: '',
        address: '',
        id_store: '',
      },
      detail_Store: {},
      defaultItem: {
        store_name: '',
        detail: '',
        phone_number: '',
        address: '',
        id_store: '',
      },

      headers2: [
        {
          text: 'ชื่อสินค้า',
          align: 'start',
          sortable: false,
          value: 'product_name',
        },
        { text: 'รายละเอียดสินค้า', value: 'detail' },
        { text: 'ราคาสินค้า', value: 'price' },
        { text: 'หน่วยสินค้า', value: 'unit' },
        { text: 'ตัวเลือก', value: 'actions' },
      ],
      products: [],
      productForm: {
        product_name: '',
        detail: '',
        price: '',
        unit: '',
        id_product_cate: '',
      },
      defaultItem2: {
        product_name: '',
        detail: '',
        price: '',
        unit: '',
        id_product_cate: '',
      },
      productsCate: [],
      id: '',
      id_store: '',
    }
  },
  watch: {
    dialog(val) {
      val || this.close()
    },
  },
  mounted() {
    this.getStoreData()
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'เพิ่มร้านค้า' : 'แก้ไขร้านค้า'
    },
  },
  methods: {
    async getProductCate() {
      try {
        const { data } = await this.$axios.get('api/v1/product/productCate')

        this.productsCate = data.data
      } catch (error) {}
    },
    async getStoreData() {
      try {
        const { data } = await this.$axios.get('api/v1/store')

        this.store = data.data
      } catch (error) {}
    },
    async getProductInStore(item) {
      let id = item.id
      try {
        let { data } = await this.$axios.get(`api/v1/product/store/${id}`)

        this.products = data.data
        // console.log(this.products)
      } catch (error) {
        // this.products = []
      }
    },
    close() {
      this.dialog = false
      this.dialog2 = false
      this.dialog3 = false
      this.$nextTick(() => {
        this.formadd = Object.assign({}, this.defaultItem)
        this.productForm = Object.assign({}, this.defaultItem2)
        this.editedIndex = -1
        this.editedIndex2 = -1
      })
    },
    editItem(item) {
      this.editedIndex = this.store.indexOf(item)
      this.formadd = Object.assign({}, item)
      this.dialog = true
    },

    editProduct(item) {
      this.editedIndex2 = this.store2.indexOf(item)
      this.productForm = Object.assign({}, item)
      this.dialog3 = true
    },
    async detailStore(item) {
      try {
        const { data } = await this.$axios.get(`api/v1/store/${item.id}`)
        this.detail_Store = data.data
      } catch (err) {}
      this.dialogDelete = true
    },
    async createProduct(id) {
      const productData = {
        product_name: this.productForm.product_name,
        detail: this.productForm.detail,
        price: parseInt(this.productForm.price),
        unit: this.productForm.unit,
        id_store: id,
        id_product_cate: this.productForm.id_product_cate,
      }
      // console.log(productData);
      try {
        await this.$axios.post(`api/v1/product`, productData)

        let id = productData.id_store
        try {
          const { data } = await this.$axios.get(`api/v1/product/store/${id}`)

          this.products = data.data
          this.dialog2 = false
          this.$nextTick(() => {
            this.productForm = Object.assign({}, this.defaultItem2)
            this.editedIndex2 = -1
          })
        } catch (err) {}
      } catch (err) {}
    },
    async save(id) {
      if (this.formTitle == 'เพิ่มร้านค้า') {
        const StoreData = {
          store_name: this.formadd.store_name,
          detail: this.formadd.detail,
          phone_number: this.formadd.phone_number,
          address: this.formadd.address,
        }
        try {
          const { data } = await this.$axios.post(`api/v1/store`, StoreData)
          this.getStoreData()
          this.dialog = false
        } catch (err) {}
      } else {
        // console.log(2);
        const StoreData = {
          store_name: this.formadd.store_name,
          detail: this.formadd.detail,
          phone_number: this.formadd.phone_number,
          address: this.formadd.address,
        }
        try {
          const { data } = await this.$axios.put(
            `api/v1/store/${id}`,
            StoreData
          )
          this.getStoreData()
          this.dialog = false
        } catch (err) {}
      }
    },
    deleteItem(item) {
      this.id = item.id
      this.id_store = item.id_store
      this.dialogDelete2 = true
    },
    async deleteItemConfirm(id, id_store) {
      console.log(id)
      try {
        await this.$axios.delete(`api/v1/product/store/${id}`, {
          method: 'delete',
        })
        try {
          const { data } = await this.$axios.get(
            `api/v1/product/store/${id_store}`
          )
          this.products = data.data
          this.dialogDelete2 = false
        } catch (error) {}
      } catch (error) {}

      // this.desserts.splice(this.editedIndex, 1)
      // this.closeDelete()
    },
    closeDelete() {
      this.dialogDelete2 = false
      this.$nextTick(() => {
        this.productForm = Object.assign({}, this.defaultItem2)
        this.editedIndex2 = -1
      })
    },
    async updateProduct(productForm) {
      let id = productForm.id
      let id_store = productForm.id_store
      const productData = {
        product_name: this.productForm.product_name,
        detail: this.productForm.detail,
        price: parseInt(this.productForm.price),
        unit: this.productForm.unit,
        id_store: id_store,
        id_product_cate: this.productForm.id_product_cate,
      }
      try {
        await this.$axios.put(`api/v1/product/store/${id}`, productData)
        try {
          const { data } = await this.$axios.get(
            `api/v1/product/store/${id_store}`
          )
          this.products = data.data
          this.dialog3 = false
        } catch (error) {}
      } catch (err) {}
    },
  },
}
</script>
<style>
hr.style5 {
  background-color: #fff;
  border-top: 2px dashed #8c8b8b;
}
</style>
