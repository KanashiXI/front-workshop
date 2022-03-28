<template>
  <v-container>
    <v-row>
      <!-- <pre>{{apidata}}</pre> -->
      <v-col>
        <v-card>
          <v-row>
            <v-col cols="6">
              <v-img src="https://riahsoftware.com/wp-content/uploads/2019/06/System-Programmer.jpg" height="200px" />
            </v-col>
            <v-col cols="6">
              <v-card-title>{{apidata.products_name}}</v-card-title>
              <v-card-title>{{apidata.price}} ฿</v-card-title>
              <v-btn depressed @click="minus()"> - </v-btn> {{num}} <v-btn depressed @click="plus()"> + </v-btn>
              <br><br>
              <v-btn depressed> cart </v-btn>
            </v-col>
          </v-row>
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
      apidata: {},
      postdata: {
        products_name: '',
        price: '',
        amout: ''
      },
      postdefault: {
        products_name: '',
        price: '',
        amout: ''
      },
      num: 0
    }
  },
  created () {
    // this.callGet()
    // EventBus.$on('detailEvent', this.detail)
    // this.detail()
    this.id = this.$route.query.id
    this.detail()
  },
  mounted () {
    // this.callGet()
    // EventBus.$on('detailEvent', this.detail)
    console.log(this.$route)
    this.id = this.$route.query.id
  },
  methods: {
    callGet () {
      this.axios.get('http://localhost:3000/products/').then((response) => {
        this.apidata = response.data.data
      })
    },
    async detail () {
      // this.apidata = {}
      // this.apidata = await this.axios.get('http://localhost:3000/products/' + item)
      // console.log(this.apidata)
      this.axios.get('http://localhost:3000/products/' + this.id).then((response) => {
        this.apidata = response.data.data
      })
      // this.axios.get('http://localhost:3000/products/' + item).then((response) => {
      //   console.log(response.data.data)
      //   this.apidata = response.data.data
      // })
    },
    minus () {
      console.log('click -')
    },
    plus () {
      console.log('click +')
    }
  }
  // beforeDestroy () {
  //   console.log('beforeDestroy')
  //   EventBus.$off('detailEvent')
  // }
}
</script>
