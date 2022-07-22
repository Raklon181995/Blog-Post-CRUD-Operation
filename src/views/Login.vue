<template>
<div>
<div align="center">
    <v-card class="mt-10" width="50%">
    <div class="text-center pt-6 h3">
       <h2> Login</h2>
    </div>

    <v-text-field
    v-model="email"
    label="Email *"
    type="email"
    class="mx-10"
    required>
    </v-text-field>
     <v-text-field
    v-model="password"
    label="Password *"
    class="mx-10"
    required>
    </v-text-field>
   
    <div class="py-6">
         <v-btn type="button" v-on:click="login()">Submit</v-btn>
    </div>
    </v-card>
</div>
</div>
</template>
<script>
export default {
    name: "login",
    data() {
        return{
            email: " ",
            password: " ",

        }
    },
    methods: {
        // login(value){
        //     if(/^\W+([\.-]?\W +()). test.value){
        //         this.msg["email"] = "";
        //     }
        //     else{
        //         this.msg["email"]
        //     }

        // }
    },
}

</script>
