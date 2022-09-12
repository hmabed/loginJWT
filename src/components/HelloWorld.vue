<script setup>
  import { ref } from 'vue'
  
  import { createClient } from '@supabase/supabase-js'
  import { SupabaseAuthClient } from '@supabase/supabase-js/dist/module/lib/SupabaseAuthClient'
  
  defineProps({
    msg: String
  })
  
  const count = ref(0)
  
  supabase.auth.onAuthStateChange((event, session) => {
    if(session==null){
      document.getElementById('status').innerHTML='You are not logged !!!';
    } else{
      //alert('session value: ' + JSON.stringify(session))
      document.getElementById('status').innerHTML='You are logged with the email: ' + session.user.email;
    }
  })
  
  </script>
  
  <template>
    <h1>{{ msg }}</h1>
    <p>
      Please login if you have an account or register :
    </p>
    <label>email: </label><br>
    <input type="email" required v-model="email" placeholder="username@domain.tld"> <br>
    <label>password: </label><br>
    <input type="password" required v-model="passwd" ><br>
    <button @click="register()">Sign Up</button>
    <button @click="login()">Sign In</button><br>
    <label id="status">You are not yet logged !  </label>
  </template>
<script>
  const supabaseUrl = 'https://<YOUR_APP_REF.supabase.co'
  const SUPABASE_KEY = 'YOUR_ANON_KEY'
  const supabase = createClient(supabaseUrl, SUPABASE_KEY)
  export default {
    data() {
      return {          
      }
    },
    methods: {  
      //this method allows a new user to sign up the system. Once done, the user receives an email
      //asking for account validation. Once the validation made the user is added to the system
      async register(){
        try {
          const { user, session, error } = await supabase.auth.signUp({
            email: this.email,
            password: this.passwd,
          });
          if (error) throw error;
          document.getElementById('status').innerHTML='Please validate the received email !'
        } catch (error) {
          alert(error.error_description || error.message);
        } 
      },
      //this method allows the already registred user to log in the system.
      //only authenticated users can later add or read the poems
      
      async login(){
        try {
          const { user, session, error } = await supabase.auth.signIn({
            email: this.email,
            password: this.passwd,
          });
          if (error) throw error;
          document.getElementById('status').innerHTML='You are now logged !'
        } catch (error) {
          alert(error.error_description || error.message);
        } 
      }
    }
  }
  
  </script>
  


<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
