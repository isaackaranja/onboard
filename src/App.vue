<template>
  <div >
    
    <v-app id="inspire" class="fold">
      <Header></Header>
      <v-form v-model="valid">
        <v-container>
          <v-row>
            <v-col
              cols="12"
              sm="6"
              md="6"
            >
              <v-text-field
                v-model="venueName"
                label="Venue Name"
                placeholder="Venue"
                outlined
                :rules="rules"
                dense
                required
              ></v-text-field>
            </v-col>
            <v-col
              cols="12"
              sm="6"
              md="6"
            >
              <v-text-field
                v-model="ownerName"
                label="Owner Name"
                placeholder="Owner"
                :rules="rules"
                outlined
                dense
                required
              ></v-text-field>
            </v-col>
            <v-col
              cols="12"
              sm="6"
              md="6"
            >
              <v-text-field
                v-model="telephoneNumber"
                label="Telephone Number"
                placeholder="T No"
                outlined
                :rules="rules"
                dense
                required
              ></v-text-field>
            </v-col>
            <v-col
              cols="12"
              sm="6"
              md="6"
            >
              <v-text-field
                v-model="email"
                label="Email"
                placeholder="Email"
                outlined
                :rules="rules"
                dense
              ></v-text-field>
            </v-col>
            <v-col
              cols="12"
              sm="6"
              md="6"
            >
              <v-text-field
                v-model="emailAddressBilling"
                label="Email Address - Billing"
                placeholder="Email Address - Billing"
                outlined
                :rules="rules"
                dense
              ></v-text-field>
            </v-col>
            <v-col
              cols="12"
              sm="6"
              md="6"
            >
              <v-text-field
                v-model="streetAddress"
                label="Street Address"
                placeholder="Street Address"
                outlined
                :rules="rules"
                dense
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="6">
              <v-combobox
              :items="items.map(item => {return item.label})"
              :item="items.map(item => {return item.label})"
              v-model="select"
              label="POS Provider"
              outlined
              :rules="rules"
              dense
              required
            ></v-combobox>
            </v-col>
             <v-col cols="12" sm="6" md="6">
              <v-combobox
              :items="venue_types.map(item => {return item.label})"
              :item="venue_types.map(item => {return item.label})"
              v-model="venue_type_select"
              label="Venue Type"
              outlined
              dense
              required
            ></v-combobox>
            </v-col>
          </v-row>
           <Drink v-model="available_drinks" :manufacturers="manufacturers" :all_drinks="allDrinks"/>
        </v-container>
        <v-container>
          <v-row justify="center" class="mt-4">
              <v-btn  depressed color="#3664b0" md="6" block @click="add_venue" :disabled="validate()"> Save </v-btn><br/>
          </v-row>
        </v-container>
      </v-form>
    </v-app>
    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Drink from './components/Drink.vue'
export default {
  name: 'App',
  components: {
    Header,
    Drink,
  },
  data () {
      return {
        select: undefined,
        items: [
          {val:"tabletpos", label:'TabletPos'},
          {val: "tabletpos", label: "TabletPos"},
          {val: "vectron", label: "Vectron"},
          {val: "micros", label: "Micros"},
          {val: "gaap", label: "Gaap"},
          {val: "pilot", label: "Pilot"}
        ].sort((a, b) => a.shortname > b.shortname ? 1 : -1),
        venue_type_select: undefined,
        venue_types: [
          {val: "bar", label: 'Bar'},
          {val: "club", label: "club"},
          {val: "hotel", label: "Hotel"},
          {val: "restaurant", label: "Restaurant"},
        ].sort((a, b) => a.label > b.label ? 1 : -1),
        valid: false,
        venueName: undefined,
        ownerName: undefined,
        telephoneNumber: undefined,
        email: undefined,
        streetAddress: undefined,
        emailAddressBilling: undefined,
        available_drinks: undefined,
        rules: [
          value => !!value || 'Required.',
          value => (value && value.length >= 1) || 'Min 3 characters',
        ],
        manufacturers: undefined,
        allDrinks: undefined
      }
  },
  computed: {
    results () {
      return{
        name: this.venueName, 
        owner: this.ownerName, 
        telephone: this.telephoneNumber, 
        email: this.email, 
        emailBilling:this.emailAddressBilling, 
        streetAddress: this.streetAddress, 
        drinks: this.available_drinks,
      }
    }

  },
  methods: {
    add_venue: function () {
      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");

      var raw = JSON.stringify({
        "payload": this.results
      });

      var requestOptions = {
        method: 'POST',
        headers: myHeaders,
        body: raw,
      };

      fetch("http://api.acusense.io/apis/venue_entries", requestOptions)
        .then(response => response.text())
        .then(result => console.log(result))
        .catch(error => console.log('error', error));
    },

    validate: function () {
      let values_defined = (
        this.venueName != undefined 
        && this.ownerName != undefined 
        && this.telephoneNumber != undefined 
        && this.select.length >= 1 
        && this.email != undefined 
        && this.emailAddressBilling != undefined 
        && this.streetAddress != undefined 
        && this.venue_type_select.length != undefined
      );

      let isValid = (
        values_defined 
        && (
          this.venueName.length > 3 
          && this.ownerName.length > 3 
          && this.telephoneNumber.length > 3 
          && this.select.length > 1 
          && this.email.length > 3 
          && this.emailAddressBilling.length > 3 
          && this.streetAddress.length > 3 
          && this.venue_type_select.length > 1
          )
        );
      return !isValid
    }
  },
  mounted() {
    var myHeaders = new Headers()
    myHeaders.append("Content-Type", "application/json");
    // var requestOptions = {
    //   method: 'GET',
    //   headers: myHeaders,
    // };
    fetch("http://api.acusense.io/apis/manufacturers")
        .then(response => response.json())
        .then(result => {
          this.manufacturers = result.sort((a, b) => a.shortname > b.shortname ? 1 : -1)
        })
        .catch(error => console.log('error', error));
      
      fetch("http://api.acusense.io/apis/drinks")
      .then(response => response.json())
      .then(result => {
        this.allDrinks = result.sort((a, b) => a.shortname > b.shortname ? 1 : -1)
      })
      .catch(error => console.log('error', error))
  },
  
}
</script>

<style>
#app {
  font-family: "Asap", "Helvetica Neue", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

}
body{
  font-family: "Asap", "Helvetica Neue", Helvetica, Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #f7f7f9;

}
.container{
  width:100%;
  width:100%;
  border-radius: 5px;
  padding: 25px 30px;

}

Header{
  font-size: 25px;
  font-family: "Questrial", "Helvetica Neue", Helvetica, Arial, sans-serif;
  position: relative;
  margin-left: 24px;
}

.container Header::before{
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  height: 3px;
  width: 50px;
  background: blue;
}
.container form .venue_details{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.input-box{
    margin-bottom: 15px;
    width: calc(100% / 2 - 20px);
}
v-combobox{
  border-radius: 5px;
  border: 1px solid #ccc;
  font-size: 16px;
  padding-left: 15px;
}
.add-but{
  color: yellow;
}
.fold {
  box-shadow: 1px 1px;
}
.btn {
  display: inline-block;
  background: blue;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
/* .btn-block {
  display: block;
  width: 100%;
} */
</style>
