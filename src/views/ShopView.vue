<template>
  <v-container>
    <v-row>
      <v-col cols="3" v-for="(data, index) in apidata" :key="index">
        <v-card @click="detail(data._id)">
          <v-img src="https://riahsoftware.com/wp-content/uploads/2019/06/System-Programmer.jpg" height="200px" />
          <v-card-title>{{data.products_name}}</v-card-title>
          <v-card-subtitle>{{data.price}} ฿
            <p class="text-right">เหลือ {{data.amount}} ชิ้น</p>
          </v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
// import { EventBus } from '@/EventBus'
export default {
  data () {
    return {
      id: '',
      apidata: [],
      dialogedit: false,
      dialogDelete: false,
      postdata: {
        products_name: '',
        price: '',
        amout: ''
      },
      postdefault: {
        products_name: '',
        price: '',
        amout: ''
      }
    }
  },
  created () {
    this.callGet()
  },
  computed: {
    savemode () {
      return this.id === '' ? 'New Item' : 'Edit Item'
    }
  },
  methods: {
    detail (item) {
      console.log(item)
      // EventBus.$emit('detailEvent', item)
      this.$router.push({ path: '/detail', query: { id: item } })
    },
    newItem () {
      this.id = ''
      this.postdata = Object.assign({}, this.postdefault)
      this.dialogedit = true
    },
    editmode (item) {
      this.id = item._id
      this.postdata = Object.assign({}, item)
      this.dialogedit = true
    },
    deletemode (item) {
      this.id = item._id
      this.postdata = Object.assign({}, item)
      this.dialogDelete = true
    },
    savedata  () {
      console.log(this.savemode)
      if (this.savemode === 'Edit Item') {
        this.putData()
      } else this.postData()
    },
    callGet () {
      this.axios.get('http://localhost:3000/products').then((response) => {
        this.apidata = response.data.data
        console.log(this.apidata)
      })
    },
    async postData () {
      try {
        var { data } = await this.axios.post('http://localhost:3000/products', this.postdata)
        alert(data.message)
        this.callGet()
        this.postdata = Object.assign({}, this.postdefault)
        this.dialogedit = false
      } catch (error) {
        console.log(error.message)
      }
    },
    async putData () {
      try {
        var { data } = await this.axios.put('http://localhost:3000/products/' + this.id, this.postdata)
        alert(data.message)
        this.callGet()
        this.postdata = Object.assign({}, this.postdefault)
        this.dialogedit = false
      } catch (error) {
        console.log(error.message)
      }
    },
    async delDataOne () {
      try {
        var { data } = await this.axios.delete('http://localhost:3000/products/' + this.id)
        alert(data.message)
        this.callGet()
        this.postdata = Object.assign({}, this.postdefault)
        this.dialogedit = false
      } catch (error) {
        console.log(error.message)
      }
    }
  }
}
</script>

<style>

</style>
