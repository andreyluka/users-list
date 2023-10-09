<script>
export default {
   name: 'usersPagination',
   props: {
      amountPages: Number,
      active: {
         type: Number,
         default: 1
      }
   }
}
</script>

<template>
   <ul @click="$emit('clickPagination', $event.target.innerText)" class="pagination">
      <li :class="['pagination__item', {'disabled':active === 1}]">&laquo;</li>
      <li 
         v-for="page in amountPages"
         :key="page"
         :class="['pagination__item', {'active':page === active}]"
      >{{ page }}</li>
      <li :class="['pagination__item', {'disabled':active === amountPages}]">&raquo;</li>
   </ul>
</template>

<style scoped lang="scss">
.pagination {
   display: flex;
   justify-content: center;
   align-items: center;
   font-size: 22px;

   &__item {
      float: left;
      padding: 0.5em 1em;
      text-decoration: none;
      transition: 0.3s;
      cursor: pointer;
      
      &:hover:not(.active) {
         background-color: grey;
         border-radius: 5px;
         color: black;
      }
   }

   .active {
      background-color: var(--color-purple-light);
      border-radius: 5px;
      pointer-events: none;
   }

   .disabled {
      opacity: 0.4;
      pointer-events: none;
   }
}
</style>