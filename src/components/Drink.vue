<template>
    <div>
      <v-card>
        <div class="mb-3">
          <v-card-title class="mt-0 pd-0">Drink Brand</v-card-title>
          <v-row>
            <v-col cols="9">
              <DrinkSelection v-model="drinkSelection" :manufacturers_obj="manufacturers" :all_drinks="all_drinks" class="pb-1 mb-0"></DrinkSelection>
            </v-col>
           
            <v-col>
               <v-btn
                depressed 
                @click="drinks.unshift(drinkSelection)" 
                color="primary" class="add-button pb-0 mb-4 ml-2"
                :disabled="add_drink_button_disable"
              >
                {{add_drink_button_text}}
              </v-btn>
            </v-col>
          </v-row>
        </div>
      </v-card>

      <v-row>
        <v-col cols="12" class="mt-5" v-for="(drink, index) in drinks" :key="drink.id">
          <v-card outlined class="cad-board mb-0 pb-0">
            <DrinkPage v-model="drinks[index]" class="mb-10"/>
          </v-card>
        </v-col>
      </v-row>
    </div>

</template>
<script>
import DrinkSelection from './DrinkSelection.vue'
import DrinkPage from './DrinkPage.vue'
export default({
    name: "Drink",
    props: ["manufacturers", "all_drinks"],
    components: {
      DrinkSelection,
      DrinkPage
    },
    data: () => {
      return {
        drinkSelection: undefined,
        drinks: [],
      }
    },
    methods: {
      // handle_event(event){
      //   console.log("event:", event)
      // }
    },
    computed: {
      add_drink_button_text: function() {
        if (this.drinkSelection == undefined) {
          return "Add"
        }
        return [...new Set(this.drinks.map(item => item.id))].indexOf(this.drinkSelection.id) === -1 ? "Add" : "Exists"
      },
      add_drink_button_disable: function () {
        return !(this.drinkSelection || {})['brand'] || this.add_drink_button_text === 'Exists'
      },
      result() {
        return {
          drinks: this.drinks
        }
      }

    },
    watch: {
      result(to) {
        this.$emit("input",to)
      }
    },
})
</script>
<style scoped>
  .cad-board{
    margin-bottom: 0;
    padding-bottom: 0;
    background-color: #FFFAF0;
  }
</style>
