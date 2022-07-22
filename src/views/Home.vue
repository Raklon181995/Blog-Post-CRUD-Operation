<template>
    <div>
        <v-container>
        <div class="mt-6 text-end mr-8">
        <v-dialog
            v-model="dialog"
            width="500">
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                color="primary"
                dark
                v-bind="attrs"
                v-on="on"
                >
                    Insert
                    <v-icon dark>
                    mdi-plus
                    </v-icon>
                </v-btn>
            </template>
            
            <v-card>
                <div class="mx-4">
                    <v-flex class="py-4">
                        <v-form ref="form" class="white ma-5" v-model="valid">
                            <v-text-field class="pa-3" v-model="task" :rules="titleRules" label="Title">
                            </v-text-field>
                            <v-text-field class="pa-3" v-model="author" label="Author" :rules="nameRules">
                            </v-text-field>
                            <v-text-field class="pa-3" v-model="description" label="Content" :rules="descriptionRules">
                            </v-text-field>
                            <v-btn v-if="edit!=true" color="primary" @click="addTask(task,description,author,$event), dialog = false">
                                Add Task
                            </v-btn>
                            <v-btn v-if="edit==true" color="success" class="mr-4" @click="updateTask($event), dialog = false">
                                Update
                            </v-btn>    
                            <v-btn v-if="edit==true" color="error" @click="cancelTask($event), dialog = false">
                                Cancel
                            </v-btn>
                        </v-form>
                    </v-flex>
                </div>
            </v-card>
        </v-dialog>
    </div>
        <div row class="mt-8" align="center">
            <v-flex >
                <v-flex xs12 
                v-for="(task, index) in tasks"
                :key="index"
                >
                   <v-card  class="mb-3" flat
                    :width="
                    $vuetify.breakpoint.xs || $vuetify.breakpoint.sm ? '100%' : '70%'">
                        <v-card-title primary-title>
                        <div class="text-start">
                            <div class=d-flex>
                                <div class="pt-1">
                                   <img src="@/assets/account.svg" alt="hamburger"  width="20px"/>
                                </div>
                                <div class="font-14 gray-text ml-2">Rakesh Lonikar</div>
                                <div class="font-14 gray-text ml-2"> . July 22, 2022</div>
                            </div>
                            <div class="headline mb-0 font-30">{{task.task}}</div>
                            <div class="font-16">{{task.description}}</div>
                            <div class="font-14 text-end mt-4">Author: {{task.author}}</div>
                        </div>
                    </v-card-title>
                    <v-card-actions v-if="edit==false"
                    class="justify-center py-4">
                        <v-btn color="success" width="100px" @click="editTask(task,index)">
                            <v-icon left> mdi-pencil</v-icon>
                            Edit
                        </v-btn>
                         <v-btn color="error" class="ml-4" width="100px" @click="deleteTask(index)">
                             <v-icon left> mdi-delete</v-icon>
                             Delete</v-btn>
                    </v-card-actions>

                    <v-card-actions v-if="edit!=false">
                        <v-btn disabled color="warning">Edit</v-btn>
                         <v-btn disabled color="error">Delete</v-btn>
                    </v-card-actions>
                     <v-divider></v-divider>
                   </v-card>
                </v-flex>

            </v-flex>
        </div>
        </v-container>
    </div>
</template>
<script>

  export default {
    components:{
    },
  
    data: () => ({
        dialog: false,
        task:'',
        description:'',
        author: '',
        edit:false,
        delete:false,
        cancel:false,
        id2:0,
        ind:0,
        tasks:[],
      valid: true,
      nameRules: [
        v => !!v || 'Author Name is required',
      ],
      titleRules: [
        v=> !!v || 'Title is required',
      ],
       descriptionRules: [
        v=> !!v || 'Content is required',
      ],   
    }),
    methods:{
        validate () {
        this.$refs.form.validate()
      },
        addTask:function(t,d,a,e){
            e.preventDefault();
            this.tasks.push({
                task:this.task,
                description:this.description,
                author: this.author
            });
            localStorage.setItem('tasks',JSON.stringify(this.tasks));
            this.task='';
            this.description='';
            this.author='';
            this.$refs.form.validate()
        },
        editTask:function(t,i){
            this.edit=!this.edit;
            this.task=t.task;
            this.description=t.description;
            this.author=t.author;
            this.ind=i;
        },
        updateTask:function(e){
            e.preventDefault();
            this.edit=!this.edit;
            let taskdb={
                task:this.task,
                description:this.description,
                author:this.author,
            }
            this.tasks[this.ind]=taskdb;
            localStorage.setItem('tasks',JSON.stringify(this.tasks));
            let taskDB = JSON.parse(localStorage.getItem('tasks'));
            this.tasks= taskDB;
            this.task='';
            this.description='';
            this.author='';
        },
        cancelTask:function(e){
            e.preventDefault();
            this.task='';
            this.description='';
            this.author='';
            this.edit=!this.editTask;
        },
        deleteTask: function(i){
            this.tasks.splice(i,1);
            localStorage.setItem('tasks', JSON.stringify(this.tasks));
        }
    },
    created:function(){
        let taskDB= JSON.parse(localStorage.getItem('tasks'));
        if(taskDB === null){
            this.tasks=[];
        }else{
            this.tasks=taskDB;
        }
    }
   
}
</script>
<style lang="scss" scoped>
.font-30{
  font-size: 26px;
  font-weight: 600;
}
.font-16{
  font-size: 18px;
  font-weight: 400;
  color: gray;
}
.font-14{
    font-size: 14px;
    font-weight: 400;
}
.gray-text{
    color: gray;
}

</style>