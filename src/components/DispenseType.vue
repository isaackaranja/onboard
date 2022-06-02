<template>
	<v-combobox
		v-model="dispenseType"
		label="Dispense Type"
	></v-combobox>
</template>

<script>
  export default{
    name: "DispenseType",
    props: ['value'],
	 data() {
		 return {
			 dispenseType: ''
		 }
	 },
   watch: {
    dispenseType(to){
      console.log("type",to)
      this.$emit('dispenseType',to)
    }
   },
   
  };
</script>