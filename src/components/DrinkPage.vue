<template>
  <div>
    <v-container>
      <v-row justify="start">
        <v-col cols="8">
          <v-card-title> 
            {{ manufacturer.name }} : 
            {{ brand.name }}
          </v-card-title>
        </v-col>
        <v-col cols="2"  class="ml-auto">
          <v-card-actions>
            <v-btn small @click="add_row()" class="mt-3 pa-1" color="primary" width="100px">Add Dispense</v-btn>
          </v-card-actions>
        </v-col>
      </v-row>
    </v-container>
    <!-- :disabled="!(value.tipples[tipples.length-1] || {})[skus]" -->
    
    <div v-for="(tipple, index) in tipples" :key="tipple.id" class="ma-0 pt-2">
    <v-container>
      <v-row>
        <v-col cols=10><SkuDefinition v-model="tipples[index]" class="mb-0 pb-0"/></v-col>
        <v-col class="ml-auto"><v-btn small color="error" @click="delete_tipple(tipple.id)" class="ma-2" width="100px">delete</v-btn></v-col>
      </v-row>
    </v-container>
      
    </div>
  </div>
</template>
<script>
import SkuDefinition from './SkuDefinition'
export default ({
  name: "DrinkPage",
  components: {
    SkuDefinition,
  },
  props: ['value'],
  data() {
    return {
      manufacturer: undefined,
      brand: undefined,
      tipples: undefined,
    }
  },
  computed: {
    result() {
      return {
        manufacturer: this.manufacturer,
        brand: this.brand,
        tipples: this.tipples,
        id: `${this.manufacturer ? this.manufacturer['shortname'] : undefined}_${this.brand ? this.brand.name : undefined}`.replace(/ /gi, "_"),
      }
    }
  },
  watch: {
    result(to) {
      this.$emit('input',to);
    }
  },
  methods: {
    add_row() {
      this.tipples.push({id:this.tipples.length,type: undefined, size: undefined, skus: undefined});
    },
    delete_tipple(id) {
      this.tipples.splice(id, 1)
    }
  },
  mounted() {
    this.manufacturer = this.value.manufacturer;
    this.brand = this.value.brand;
    this.tipples = this.value.tipples || [{id:0}];
  }
})
</script>
