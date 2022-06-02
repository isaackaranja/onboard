<template>
	<v-combobox
    :items="sku_select"
		v-model="sku"
		label="Sku"
	></v-combobox>
</template>

<script>
  export default{
    name: "Sku",
    props: ['value'],
	 data() {
		 return {
			sku: '',
      sku_select: ['pint', 'half_pint', 'jag']
		 }
	 },
   watch: {
    sku(to){
        console.log(to)
        this.$emit('sku',to)
      }
   },
   
  };
</script>