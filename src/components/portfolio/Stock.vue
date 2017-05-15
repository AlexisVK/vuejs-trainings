<template>
  <div class="col-sm-6">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h3 class="panel-title">{{ stock.name }} <small>(Price: {{ stock.price | currency }} | Quantity: {{ stock.quantity }}</small></h3>
      </div>
      <div class="panel-body">
        <div class="pull-left" :class="{'has-warning': insufficientQuantity}">
          <input type="number" placeholder="Quantity" class="form-control" v-model="quantity">
        </div>
        <div class="pull-right">
          <button class="btn btn-success" @click="sellStock" :disabled="insufficientQuantity || disabledCondition">Sell</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {mapActions} from 'vuex';
  export default {
    props: ['stock'],
    data() {
      return {
        quantity: 0
      }
    },
    computed: {
      disabledCondition() {
        return this.quantity <= 0 || !Number.isInteger(+this.quantity);
      },
      insufficientQuantity() {
        return this.quantity > this.stock.quantity;
      }
    },
    methods: {
      ...mapActions({
        placeSellOrder: 'sellStock'
      }),
      sellStock() {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        }
        this.placeSellOrder(order);
        this.quantity = 0;
      }
    }
  }
</script>