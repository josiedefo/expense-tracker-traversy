<template>
     <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions" 
        :key="transaction.id" 
        :class="{ minus: transaction.amount < 0, plus: transaction.amount > 0 }">
          {{ transaction.text }} <span>{{ transaction.amount < 0 ? '-' : '+' }}${{ Math.abs(transaction.amount) }}</span>
          <button class="delete-btn">x</button> 
          </li>
        <!--<li class="minus">
          Cash <span>-$400</span><button class="delete-btn">x</button>
        </li>
        <li class="plus">
          Paycheck <span>$800</span><button class="delete-btn">x</button>
        </li>-->
      </ul>
</template>

<script>
export default {
  data() {
    return {
      transactions: [
        { id: 1, text: 'Cash', amount: -400 },
        { id: 2, text: 'Paycheck', amount: 800 },
        { id: 1, text: 'Flower', amount: -19.99 },
        { id: 1, text: 'Camera', amount: 150.99 },
      ],
    };
  },
};
</script>