<template>
 <v-main class="listTUGAS">
   <h3 class="text-h3 font-weight-medium mb-5">To Do List TUGAS</h3>

   <v-card>
     <v-card-title>
       <v-text-field
         v-model="search"
         append-icon="mdi-magnify"
         label="Search"
         single-line
         hide-details
       ></v-text-field>
       <v-col
        class="d-flex"
        cols="12"
        sm="6"
      >
        <v-select
         v-model="search"
          :items="['All priority', 'Penting', 'Biasa', 'Tidak penting']"
          label="Priority"
          outlined
        ></v-select>
      </v-col>
       <v-spacer></v-spacer>
       <v-btn color="success" dark @click="dialog = true">
         Tambah
       </v-btn>
     </v-card-title>
     <v-data-table :headers="headers" :items="todos" :search="search">
       <template v-slot:[`item.actions`]="{ item }">
         <v-btn small class="mr-2" @click="editItem(item)">
           edit
         </v-btn>
         <v-btn small @click="deleteItem(item)">
           delete
         </v-btn>
       </template>
       <template v-slot:[`item.checkbox`]="{ item }">
          <input type="checkbox" @change="checking($event, item)">
       </template>
     </v-data-table>
   </v-card>
   <br>
   <v-card>
        <ul v-for="(data, index) in check" :key="index">
            <li>
                {{ data.task }}
            </li>
        </ul>
        <v-card-title>
        <v-btn v-if="check.length" @click="deleteAll()">Delete All</v-btn>
        </v-card-title>
    </v-card>

   <v-dialog v-model="dialog" persistent max-width="600px">
     <v-card>
       <v-card-title>
         <span class="headline">Form Todo</span>
       </v-card-title>
       <v-card-text>
         <v-container>
           <v-text-field
             v-model="formTodo.task"
             label="Task"
             required
           ></v-text-field>

           <v-select
             v-model="formTodo.priority"
             :items="['Penting', 'Biasa', 'Tidak penting']"
             label="Priority"
             required
           ></v-select>

           <v-textarea
             v-model="formTodo.note"
             label="Note"
             required
           ></v-textarea>
         </v-container>
       </v-card-text>
       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancel">
           Cancel
         </v-btn>
         <v-btn color="blue darken-1" text @click="save">
           Save
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>

 </v-main>
</template>

<script>
export default {
 name: "ListTUGAS",
 data() {
   return {
     search: null,
     dialog: false,
     check: [],
     checkbox: false,
     headers: [
       {
         text: "Task",
         align: "start",
         sortable: true,
         value: "task",
       },
       { text: "Priority", value: "priority" },
       { text: "Note", value: "note" },
       { text: "Actions", value: "actions" },
       { text: "", value: "checkbox"},
     ],
     todos: [
       {
         task: "bernafas",
         priority: "Penting",
         note: "huffttt",
       },
       {
         task: "nongkrong",
         priority: "Tidak penting",
         note: "bersama tman2",
       },
       {
         task: "masak",
         priority: "Biasa",
         note: "masak air 500ml",
       },
     ],
     formTodo: {
       task: null,
       priority: null,
       note: null,
     },
     editedIndex: -1,
     editedItem: {
       task: null,
       priority: null,
       note: null,
     }
   };
 },
 methods: {
   save() {
     if (this.editedIndex > -1) {
          Object.assign(this.todos[this.editedIndex], this.formTodo)
        } else {
          this.todos.push(this.formTodo)
        }
        this.resetForm();
     this.dialog = false;
   },
   cancel() {
     this.resetForm();
     this.dialog = false;
   },
   resetForm() {
     this.formTodo = {
       task: null,
       priority: null,
       note: null,
     };
   },
   editItem(item) {
        this.editedIndex = this.todos.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
   },
   deleteItem(item) {
        const index = this.todos.indexOf(item)
        confirm('Yakin ingin menghapus?') && this.todos.splice(index, 1)
   },
   deleteAll() {
      var i  ;
      for(i in this.check){
        this.todos.splice(this.check[i], 1)
      }
      this.check = []
   },
   checking(event, item) {
      if (event.target.checked){
          this.check.push(item)
      } else {
        this.check.splice(this.check.indexOf(item), 1)
      }
   }
 },
};
</script>
