<template>
  <div>
    <a-alert v-if="isSubmit" class="alert-box" message="Successfully Submit" type="success" />

    <h3>กล่องสีเขียว</h3>
    <img :src="require('~/assets/images/mockup/default-checkout-green.svg')" alt="package-image">

    <h3>รายละเอียดผู้รับการรักษา:</h3>
    <AddressOrder :detail="order.address" />

    <a-divider />

    <h3>รายละเอียดสินค้า:</h3>
    <ItemOrder :items="order.orderLines" />

    <a-row class="button-wrapper" justify="space-around" type="flex">
      <a-col span="11">
        <a-button block @click="backToAddress">
          กลับ
        </a-button>
      </a-col>

      <a-col span="2" />

      <a-col span="11">
        <a-button block type="primary" @click="submitOrder">
          ยืนยัน
        </a-button>
      </a-col>
    </a-row>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import ItemOrder from '~/components/checkout/ItemCheckout.vue'
import AddressOrder from '~/components/checkout/AddressCheckout.vue'
import CheckoutModule from '~/store/checkout.module'
import { Order } from '~/types/order.type'
import CommonModule from '~/store/common.module'

export default Vue.extend({
  components: {
    ItemOrder,
    AddressOrder
  },
  layout: 'mobile-empty',
  data () {
    return {
      isSubmit: false
    }
  },
  computed: {
    order (): Order {
      return CheckoutModule.order
    }
  },
  async mounted () {
    CommonModule.setHeaderTitle({ header: 'กรุณาตรวจสอบข้อมูล' })
    await CheckoutModule.getOrder({ id: 1 })
  },
  methods: {
    backToAddress (): void {
      this.$router.push('/checkout')
    },

    submitOrder (): void {
      this.isSubmit = true
      // hide alert box
      setTimeout(() => {
        this.isSubmit = false
      }, 3000)
    }
  }
})
</script>

<style scoped>
.button-wrapper {
  margin: 20px 0;
}

.ant-divider {
  height: 2px;
}

.ant-alert.ant-alert-success {
  background: #096F5A;
  margin-bottom: 8px;
}

h3 {
  margin: 10px 0;
}
</style>

<style>
.alert-box .ant-alert-message {
  color: white;
}
</style>
