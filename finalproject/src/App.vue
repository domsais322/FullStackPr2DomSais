<template id="main">
  <v-app>
    
    <v-app-bar
      app
      color="primary"
      dark
    >
    <v-row>
      <v-col cols="5"
          offset="5">
      <v-icon>mdi-format-list-bulleted</v-icon>
          FRAMEWORKS
      

      </v-col>
      <v-col>
        <v-btn class="primary" dark  @click="adderDialog(),showDialog()">
            <v-icon>mdi-plus-circle</v-icon>
            Add</v-btn>
      <AddDialog @addedTask="addItem" @editerTasker="editItem" @closerDialog="closeDialog" :title="title" :open="open" :updateDialog="updateDialog"   ref="addialog"></AddDialog>
    </v-col>
    
    </v-row>
    </v-app-bar>

    <v-main>
     
      <v-container>
        <v-simple-table> 
          <thead>
            <tr>
              <th> Title</th>
              <th> Description</th>
              <th > Deadline</th>
              <th > Priority</th>
              <th > Is Complete</th>
              <th > Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item, index in rowData"
                       :key = "item.id">
            <td  > {{item.title}}</td>
            <td >  {{item.description}}</td>
            <td >  {{item.deadline}}</td>
            <td >  {{item.priority}}</td>
            <td><v-checkbox v-model="checkbox"  @click="checked()"></v-checkbox></td>
            <td > <v-btn class="primary" @click="updaterDialog(index),showDialog()" v-if=!checkbox>
              <v-icon> mdi-square-edit-outline</v-icon>
              Update</v-btn>
               <v-btn class="error" @click="deleteRow(index)">
              <v-icon>mdi-cancel</v-icon>
              Delete</v-btn> </td>
            </tr>
          
          </tbody>
        </v-simple-table>
        
      </v-container>
     
     <v-snackbar
      v-model="snackbar"
      timeout=3000
      :color="snackColor"
      right
    >{{ snackText }}</v-snackbar>
    </v-main>
    
  </v-app>
  
</template>

<script>
//import HelloWorld from './components/HelloWorld';
//import { stringify } from 'querystring';
import AddDialog from './components/AddDialog.vue';



export default {
  name: 'App',

  components: {
    //HelloWorld,
    AddDialog,
    
    
    
  },
  
  props: {
    tasks: {rowData: []},
    title: String,
    description: String,
    deadline: String,
    priority: String,
    isComplete: String,
    action: String,


  },

  data: () => ({
    rowData: [],
    open: false,
    checkbox: false,
    voff: true,
    indexEdit: '',
    snackbar: false,
    snackColor: '',
    snackText: '',
    updateDialog:false,
    input:{
      title:'',
      description:'',
      deadline:'',
      priority:''
    },
    
    
    
    
  }),

  methods:{
    
    addItem(title, description, deadline, priority){
      
      
      this.rowData.push({
        title: title,
        description: description,
        deadline: deadline,
        priority: priority,
        
      });
      this.snackColor='success'
      this.snackText = 'This task was added successfully!'
      this.snackbar=true
      console.log(this.rowData[0])
      
      
    },
    editItem(title, description, deadline, priority){
      console.log('editItem')
      console.log(this.editIndex)
      
      this.rowData[this.editIndex].description = description
      this.rowData[this.editIndex].deadline = deadline
      this.rowData[this.editIndex].priority = priority
      this.snackColor='success'
      this.snackText = 'This task was updated successfully!'
      this.snackbar=true
      // this.rowData.push({
      //   title: title,
      //   description: description,
      //   deadline: deadline,
      //   priority: priority,
        
      // });
      
      console.log(this.rowData[0])
      
      
    },
    deleteRow(index){
        this.rowData.splice(index,1);
        this.snackColor='error'
      this.snackText = 'This task was deleted successfully!'
      this.snackbar=true
    },
    showDialog(){
      this.open=true
    },
    closeDialog(){
      this.open=false
    },

    adderDialog(){
      console.log("yeah")
      this.updateDialog=false
      //this.addDialog=true
    },
    updaterDialog(index){
      console.log("yeah")

      this.updateDialog=true
      this.editIndex=index
    },
    checked(){
      if(this.checbox==true){
        this.checbox==false
      }
      if(this.checbox==false){
        this.checbox==true
      }

    }
  },
};
</script>
