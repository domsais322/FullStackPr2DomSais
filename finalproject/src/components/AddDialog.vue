<template>
    
      <v-dialog v-model="dialog" width="350px">
        
        <v-card>
          <v-card-title class="primary">
            <h4 class="white--text">
              <v-icon color="white"> mdi-{{icon}}</v-icon>
              {{taskType}} Task</h4>
        </v-card-title>
            <v-card-text>
                <v-form class = "px-3" ref="form">
                    <v-text-field label="Title" v-model="title" :rules="inputRulesTitle" :title = "title" v-if=typeDialog> </v-text-field>
                    <v-text-field label="Description" v-model="description" :rules="inputRulesDes" :description = "description" > </v-text-field>
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
                    <v-btn class="primary mx-0 mr-3" @click="addTask()" >
                      <v-icon>mdi-{{icon}} </v-icon>
                      {{taskType}}</v-btn>
                    <v-btn class="error"  @click="dialog = false,closerDialog() ">
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
  props:{
    addDialog: Boolean,
    open: Boolean,
    updateDialog: Boolean,
  },
    
    data(){
        return{
            dialog: this.open,
            title:'',
            description:'',
            datepicker: '',
            priority: '',
            icon: '',
            typeDialog: true,
            taskType:'',
            due: null,
            submitRules: [],
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
  watch:{
    //  'addDialog' : function(){
    //   if(this.updateDialog==false){
    //      this.typeDialog=false
    //      console.log('add')
    //   }
    //  },
   
    //  'updateDialog' : function(){
    //   if(this.updateDialog==true){
    //      this.typeDialog=true
    //      console.log('up')
    //  }
     //},
    'open' : function(){
      console.log(this.open)
      console.log(this.
      typeDialog)
        this.dialog=this.open
        if(this.updateDialog==false){
          this.typeDialog=true
          this.taskType = 'Add'
          this.icon = 'plus-circle'
        }
        if(this.updateDialog==true){
          this.typeDialog=false
          this.taskType = 'Edit'
          this.icon = 'square-edit-outline'
        }

    },
  },

    methods:{
        addTask(){
            if(this.typeDialog==true){
            if(this.$refs.form.validate()){
                this.$emit('addedTask', this.title, this.description, this.due, this.priority)
                this.$emit('closerDialog')
                this.title=''
            this.description=''
            this.due=''
            this.priority=''
            }
          }
          if(this.typeDialog==false){
            
            if(this.$refs.form.validate()){
              console.log('add task to edit works')
                this.$emit('editerTasker', this.title, this.description, this.due, this.priority)
                this.$emit('closerDialog')
                this.title=''
            this.description=''
            this.due=''
            this.priority=''
            this.typeDialog=true
            }
          }
            
        },

        editTask(){
            console.log('childedit')
            if(this.$refs.form.validate()){
                this.$emit('editerTask', this.title, this.description, this.due, this.priority)
                this.$emit('closerDialog')
                this.title=''
            this.description=''
            this.due=''
            this.priority=''
            }
            
        },

        closerDialog(){
          this.$emit('closerDialog')
        }
        
        
    },
    
    

    computed:{
        formattedDate(){
            return this.due ? format(this.due, 'Do MMM YYY') : ''
        }
    },

}

</script>