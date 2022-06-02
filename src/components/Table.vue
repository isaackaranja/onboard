<template>
  <div>
    <v-col
      cols="12"
      sm="6"
      md="6"
    >
        <v-combobox
        :items="liquids"
        label="Brand"
        outlined
        dense
      ></v-combobox>
    </v-col>
    <v-btn color="primary" @click="add_row()">
        Add New
    </v-btn>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">
              dispense_type
            </th>
            <th class="text-left">
              dispense_size
            </th>
            <th class="text-left">
              pos_codes
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.id">
            <td><v-col
            class="d-flex"
            cols="12"
            sm="6"
          >
            <v-select
              :items="types"
              label="Despense Type"
              dense
            ></v-select>
          </v-col></td>
          <td><v-col
            class="d-flex"
            cols="12"
            sm="6"
          >
            <v-select
              :items="size"
              label="Dispense Size"
              dense
            ></v-select>
          </v-col></td>
          <v-combobox
              :items="item.pos_codes"
              label="POS Code"
              multiple
              outlined
              dense
            ></v-combobox>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

  </div>
      
</template>
<script>
export default{
    name: "Table",
    data: () => ({
      types: ['pint', 'half-pint', 'jag'],
      size: ['550ml', '350ml', '1550ml'],
      default_tds: {
          despense_type: 'others',
          despense_size: '',
          pos_codes:     ''
      },
      items: [],
      liquids: ["33 Degrees South IPA",
       "Acid Queen", "Amber Weiss", 
       "Atlantic Weiss", "Bings Brew", 
       "Black Label",],
       select_drinks: []

    }),
    created() {
        this.initialize()
    },
    methods: {
        initialize() {
            this.items = [
            {dispense_type: 'pint', dispense_size:'550ml', pos_codes:['sku1', 'sku2','sku3']},
            {dispense_type: 'half-pint', dispense_size:'350ml', pos_codes:['sku1', 'sku2','sku3']},
            {dispense_type: 'jag', dispense_size:'1550ml', pos_codes:['sku1', 'sku2','sku3']}
            ]
        },
        add_row() {
          this.items.push(this.default_tds)
        }
    }

}
</script>

