<template>
    <div>
        <v-row class="pa-0 ml-2">
          <v-col cols="6">
              <v-combobox
              v-model="manufacturerName"
              :items="manufacturers_obj"
              item-text="shortname"
              item-value="id"
              label="Manufacture"
              outlined
              class="manufacture"
              dense
              persistent-hint
              return-object
              single-line
              @change="()=> {brandName=undefined}"
          ></v-combobox>
        </v-col>
        <v-col cols="6">
          <v-combobox
              v-model="brandName"
              :items="drinkChoice"
              item-text="name"
              label="Brand"
              outlined
              :rules="rules"
              dense
              class="brand"
          ></v-combobox>
        </v-col>
     
        </v-row>
    </div>
</template>
<script>
export default({
    name: "DrinkSelection",
    props: ["manufacturers_obj", "all_drinks"],
    data() {
        return{
            drinkChoice: undefined,
            manufacturers: undefined,
            manufacturerName: undefined,
            brandName: undefined,
             rules: [
              value => !!value || 'Required.',
            ],
        }
    },
    methods: {
       
    },
    computed:  {
      result () {
        return {
          manufacturer: this.manufacturerName, 
          brand: this.brandName, 
          id: `${(this.manufacturerName || {})['shortname']}_${(this.brandName || {})['name']}`.replace(/ /gi, "_")
        }
      }
    },
    watch: {
      result(to){
        this.$emit('input',to)
      },
      manufacturerName(to) {
        this.drinkChoice = this.all_drinks.filter(item => item.manufacturer === to.id)
      }
    },

})
</script>

<style scoped>
</style>