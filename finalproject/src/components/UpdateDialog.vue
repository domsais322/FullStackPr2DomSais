<template>
    
    <v-dialog v-model="dialog" width="350px">
      <template v-slot:activator="{ on }">
        <v-btn id=header class="primary" dark v-on="on">
          <v-icon>mdi-plus-circle</v-icon>
          Add</v-btn>
      </template>
      <v-card>
        <v-card-title class="primary">
          <h4 class="white--text">Add Task</h4>
      </v-card-title>
          <v-card-text>
              <v-form class = "px-3" ref="form">
                  <v-text-field label="Title" v-model="title" :rules="inputRulesTitle" :title = "title"> </v-text-field>
                  <v-text-field label="Description" v-model="description" :rules="inputRulesDes" :description = "description"> </v-text-field>
                  <v-menu max-width="290">
                      <template v-slot:activator="{ on }">
                          <v-text-field :rules="inputRulesDate" :value="due" label="Deadline"  append-icon="mdi-calendar-range" v-on="on"  :due="datepicker"></v-text-field>
                      </template>
                      <v-date-picker v-model="due"></v-date-picker>
                  </v-menu>
                  <v-radio-group
   v-model="priority"
  :priority="priority">
  <v-row>
      <v-col>
    <v-radio
      label="Low"
      value="Low"
      
    ></v-radio>
</v-col>
<v-col>
    <v-radio
      label="Med"
      value="Medium"
      
    ></v-radio>

</v-col>
<v-col>
    <v-radio
      label="High"
      value="High"
      
    ></v-radio>
  </v-col>
    </v-row>
  </v-radio-group>
                  <v-btn class="primary mx-0 mr-3" @click="addTask()"  >Add</v-btn>
                  <v-btn class="error"  @click="dialog = false">
                    <v-icon>mdi-cancel</v-icon>
                    cancel</v-btn>
                  <!--<v-btn @click="tester()"></v-btn>>-->
     
              </v-form>
          </v-card-text>
      </v-card>
    </v-dialog>
  
</template>

<script>
import format from 'date-fns/format'
export default{
  
  data(){
      return{
          dialog: false,
          title:'',
          description:'',
          datepicker: '',
          priority: '',
          due: null,
          inputRulesTitle:[
              v => v && v.length > 0 || 'Title is required!'
          ],
          inputRulesDes:[
              v => v && v.length > 0 || 'Description is required!'
          ],
          inputRulesDate:[
              v => v && v.length > 0 || 'Date is required!'
          ],

      }
  },

  methods:{
      addTask(){
          
          if(this.$refs.form.validate()){
              this.$emit('addedTask', this.title, this.description, this.due, this.priority)
              this.dialog = false
          }
          this.title=''
          this.description=''
          this.due=''
          this.priority=''
      },
      updateDialog(){
        
      }
      
      
  },
  
  

  computed:{
      formattedDate(){
          return this.due ? format(this.due, 'Do MMM YYY') : ''
      }
  }

}

</script>