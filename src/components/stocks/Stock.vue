<template>
  <div class="col-sm-6">
    <div class="panel panel-success">
      <div class="panel-heading">
        <h3 class="panel-title">{{ stock.name }} <small>(Price: {{ stock.price | currency }}</small></h3>
      </div>
      <div class="panel-body">
        <div class="pull-left" :class="{'has-warning': insufficientFunds}">
          <input type="number" placeholder="Quantity" class="form-control" v-model="quantity">
        </div>
        <div class="pull-right">
          <button class="btn btn-success" @click="buyStock" :disabled="insufficientFunds || disabledCondition">Buy</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
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
      funds() {
        return this.$store.getters.funds;
      },
      insufficientFunds() {
        return this.quantity * this.stock.price > this.funds;
      }
    },
    methods: {
      buyStock() {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        }

        this.$store.dispatch('buyStock', order);
        this.quantity = 0;
      }
    }
  }
</script>