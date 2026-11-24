<template>
  <div>
            <h2>{{ burger.name }}</h2>
            <img v-bind:src="burger.img" style="width:200px;height:200px">            
            <li>Innehåller<span class="allergen"> gluten</span></li>
            <li>{{ burger.kCal }} kCal</li>
                 <li>Smashed burgare med bearnaisessås</li>
            <li>serveras med gränges på tap</li>

            <button type="button" v-on:click="removeBurger">-</button>
           <span> {{ amountOrdered }} </span>
           <button type="button" v-on:click="addBurger">+</button>
    </div>
</template>

<script>

export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
data: function () {
  return {
    amountOrdered: 0,
  }
},
methods: {
    addBurger: function () {
      this.amountOrdered += 1;
     this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      });
    },
    removeBurger: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', {
          name: this.burger.name,
          amount: this.amountOrdered
        });
      }
    }
  }
}
</script>

<style scoped>

.allergen {
    font-weight: bold;
}

</style>

