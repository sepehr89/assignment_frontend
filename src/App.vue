<template>
  <v-app>
   <div class="appclass">
    <v-form>
      <v-container>
        <v-layout justify-end="true">
          <v-flex xs6>
            <v-combobox
          v-model="selectGender"
          :items="itemsgender"
          :pagination.sync="pagination"
          label="Select your gender"
        ></v-combobox>
          </v-flex>
          <v-flex xs6>
            <v-combobox
          v-model="selectAge"
          :items="itemsAge"
          label="Select your age"
        ></v-combobox>
          </v-flex>
          <v-flex xs6 justify-end="true">
            <v-btn
              color="primary"
              @click="submit"
            >submit</v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-form>
    <div style="position: fixed;
  width: 50%;
  height: 80%;
  bottom: 0px;
  right: 0;
  border: 2px solid #73AD21;overflow-y: scroll ;overflow-x: scroll">
<template>
  <v-data-table
    :headers="headers"
    :items=pokemon
     class="TFtable" 
  >
    <template slot="headerCell" slot-scope="props" style="height=100%;width=100%">
      <v-tooltip bottom>
        <template v-slot:activator="{ on }">
          <span v-on="on">
            {{ props.header.text }}
          </span>
        </template>
        <span>
          {{ props.header.text }}
        </span>
      </v-tooltip>
    </template>
    <template v-slot:items="props">
      <td>
 {{ props.item.id }}>{{ props.item.name }}</td>
    </template>
  </v-data-table>
</template>
   </div>
    </div> 
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  
  data() {
    return {
       pagination: {
          sortBy: 'name'
        },
       selectGender: 'Gender',
        itemsgender: [
          'Male',
          'Female'
          
        ],
        selectAge: 'Age',
        itemsAge: [
          '2 to 3',
          '4 to 8',
          '9 to 13',
          '14 to 18',
          '19 to 30',
          '31 to 50',
          '51 to 70',
          '71+'
        ],headers: [
          {
            text: 'Food Plan',
            align: 'center',
            value: 'name'
          }
        ],
       pokemon: [{name:null}]
        ,
      ages: null,
      selectedAge: null,
      genders: null,
      selectedGender: null,
      recommendations: null,
      directions: null,
      direction: null,
      dialog: false,
    
    };
  },
  methods: {
  async submit() {
     
       var str; 
     await axios.get(encodeURI("http://localhost:3000/recommendFood?ages="+this.selectAge+"&gender="+this.selectGender)).then(result => {
   
      this.ages =JSON.stringify(result.data);
      var jsarr=[];
       jsarr=eval('('+this.ages+')');

       var i;
       var itm=[];
      for(i=0;i<jsarr.length;i++)
       {
         var vals='Please serve '+jsarr[i].servings+' time '+jsarr[i].srvg_sz+' of '+jsarr[i].food+' in the '+jsarr[i].foodgroup+' group '+' based on following instruction:\n'
       + jsarr[i].directional;
        this.pokemon[i].name=vals;
   }
  
      }, error => {
        console.error(error);
      });
      this.pagination.descending = !this.pagination.descending;
      this.pagination.sortBy = this.headers[0].value;
    },
    
  },
  mounted() {
   axios({ method: "GET", "url": "http://localhost:3000/recommendFood?ages=14%20to%2018&gender=Male" }).then(result => {
        this.pokemon = result.data;
      }, error => {
        console.error(error);
      });
  }
};

</script>
<style scoped>
.appclass {
    background: url('https://images.unsplash.com/photo-1490818387583-1baba5e638af?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&w=1000&q=80');
    background-size: cover;
    width: 100%;
    height: 100%;
}
  @import './assets/styles/tableRows.css';
</style>