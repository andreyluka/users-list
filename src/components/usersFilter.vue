<script>
export default {
   name: 'usersFilter',
   props: {
      modelValue: String,
      emits: ['update:modelValue'],
      placeholderAttr: {
         type: String,
         default: "Поиск"
      }
   }
}
</script>

<template>
   <div class="users-filter">
      <input 
         @input="$emit('update:modelValue', $event.target.value)"
         :value="modelValue"
         :placeholder="placeholderAttr"
         autofocus="autofocus"
         type="text" 
         class="users-filter__input" 
         >
      <div @click="$emit('hideFilter')" class="users-filter__cross"></div>
   </div>
</template>

<style scoped lang="scss">
.users-filter {
   position: relative;

   &__input {
      width: 100%;
      padding-right: 1.1em;
   }

   &__cross {
      width: 1em;
      height: 0.2em;
      background: gray;
      border-radius: 5px;
      position: absolute;
      top: 49%;
      right: 2%;
      transform: rotate(45deg);
      cursor: pointer;

      &::after {
         content: '';
         width: 100%;
         height: 100%;
         background: gray;
         border-radius: 5px;
         position: absolute;
         top: 0%;
         left: 0%;
         transform: rotate(90deg);
      }
   }
}
</style>