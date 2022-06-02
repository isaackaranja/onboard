<template>
	<v-combobox
		v-model="dispenseSize"
		label="Dispense size"
	></v-combobox>
</template>

<script>
  export default{
    name: "DispenseSize",
    data() {
      return {
        dispenseSize: '',
      }
    },
    watch: {
      dispenseSize(to){
          this.$emit('size',to)
      }
    },
  };
</script>