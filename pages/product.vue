<template>
  <v-row>
    <v-col cols="12" sm="8" md="12">
      <v-data-table
        :headers="headers"
        :items="products"
        sort-by="store_name"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>ข้อมูลสินค้าทั้งหมด</v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
          </v-toolbar>
        </template>
      </v-data-table>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'ชื่อสินค้า',
          align: 'start',
          sortable: false,
          value: 'product_name',
        },
        { text: 'รายละเอียดสินค้า', value: 'detail' },
        { text: 'ราคาสินค้า', value: 'price' },
        { text: 'หน่วยสินค้า', value: 'unit' },
      ],
      products: [],
      editedIndex: -1,
      formadd: {
        product_name: '',
        detail: '',
        price: '',
        unit: '',
      },
      defaultItem: {
        product_name: '',
        detail: '',
        price: '',
        unit: '',
      },
    }
  },
  mounted(){
    this.getAllProduct()
  },
  methods: {
    async getAllProduct() {
      try {
        const { data } = await this.$axios.get('api/v1/product')

        this.products = data.data
        log
      } catch (error) {}
    },
  },
}
</script>
