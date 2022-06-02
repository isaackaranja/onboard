<template>
  <div>
    <v-row class="px-2 mt-0">
      <v-col class="ml-3 pl-4">
        <v-combobox :items="types" v-model="type" label="Types" class="pt-0 mt-0"/>
      </v-col>
      <v-col>
        <v-text-field v-model.number="size" suffix="ml" label="Size" type="number" class="pt-0 mt-0" :disabled="!type"/>
      </v-col>
      <v-col>
        <v-combobox v-model="skus" label="SKUs" multiple small-chips class="pt-0 mt-0" :disabled="!size"/>
      </v-col>
    </v-row>
    
  </div>
</template>

<script>
  export default{
    name: "SkuDefinition",
    props: ['value'],
    data() {
      return{
        types: ['Taster', 'Half Pint', 'Pint'],
        id: undefined,
        type: undefined,
        size: undefined,
        skus: undefined,
      }
    },
    methods: {
    },
    computed: {
      result() {
        return{
          id: this.id,
          type: this.type,
          size: this.size,
          skus: this.skus
        } 
      },
    },
    watch: {
      result(to){
        this.$emit("input", to)
      }
      
    },
    mounted() {
      this.id = this.value.id;
      this.type = this.value.type;
      this.size = this.value.size;
      this.skus = this.value.skus;
    }
  };
</script>