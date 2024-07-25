<template>
  <PageHeader></PageHeader>
  <Transition name="slide-fade">
    <SuccessMessage v-if="successMsg" v-model:successMsg="successMsg"></SuccessMessage>
  </Transition>
  <ErrorMessage v-if="errorMsg" v-model:errorMsg="errorMsg"></ErrorMessage>
  <main>    
    <UserTable :users="users"                             
               @showmodal="showModal"
               @deleteuser="removeUser">
    </UserTable>
    <Transition name="fade">
      <AddModel v-if="showAddModel"                  
                    :validator="validator"
                    @submituser="addUser"
                    @closemodal="closeModal">
      </AddModel>
    </Transition>
    <Transition name="fade">
      <EditModel v-if="showEditModel"
                    :user="user" 
                    :validator="validator"
                    @submituser="editUser"
                    @closemodal="closeModal">
      </EditModel>
    </Transition>
  </main>  
</template>

<script>
import PageHeader from './components/PageHeader.vue'
import SuccessMessage from './components/SuccessMessage.vue'
import ErrorMessage from './components/ErrorMessage.vue'
import UserTable from './components/UsersTable.vue'
import AddModel from './components/AddModel.vue'
import EditModel from './components/EditModel.vue'

export default {
  name: 'App',
  components: {
    PageHeader,
    SuccessMessage,
    ErrorMessage,
    UserTable,
    AddModel,
    EditModel    
  },
  data(){
    return{      
      user:false,
      users:[],
      successMsg:false,
      errorMsg:false,
      showAddModel:false,      
      showEditModel:false,
      validator: {
        name: [],            
        email: [],    
        phone: []
      }      
    }
  },
  mounted() {
    this.showUsers()
  },
  watch: {    
    'successMsg'(newValue) {
      if(newValue){setTimeout(() => this.successMsg = false, 4000)}
    },
    'errorMsg'(newValue) {
      if(newValue){setTimeout(() => this.errorMsg = false, 4000)}
    }
  },  
  methods: {
    // Get all user from database
    // return void
    showUsers ()
    {         
      fetch('http://localhost:8001')
      .then(resp => resp.json())
      .then(json => {
        if(json.status == 200){
        this.users = json.results.reverse()
      } 
      })
      .catch(error => {
          console.error(error)
      })
    },
    // Get user from database from id
    // id number
    // return void
    getOneUser(id)
    {           
      fetch('http://localhost:8001/?id=' + id)
      .then(resp => resp.json())
      .then(json => {
        if(json.status == 200){
          // assign user
          this.user = json.results[0]
          // display the modal
          this.showEditModel=true     
        }   
      })
      .catch(error => {
          console.error(error)
      })
    },
    // Create, Update, Delete record from database 
    // data Object
    // return void
    cudUser(data)
    {
      fetch('http://localhost:8001', data.options)
      .then(resp => resp.json())
      .then(json => {
        if(json.status == 201 || json.status == 200){
          //assign success message
          this.successMsg = data.success
          //refresh the user list
          this.showUsers()
        }else{
          //assign error message
          this.errorMsg = data.error
          console.error(json.message)
        }
      })
      .catch(error => {
          console.error(error)
      })
    },
    // Add user method
    // input_user Object
    // return void
    addUser(input_user)
    {      
      if(!this.validate(input_user)){
         return; 
      } 

      let data = {
        success: 'New user successfully created',
        error: 'Create user unsuccessful.',
        options :{
          method: 'POST',
          body: JSON.stringify(input_user),
          headers: {
              'Content-Type': 'application/json'
          }
        }
      }

      this.cudUser(data)  
      // close the add modal    
      this.closeModal('add')
    },    
    // Edit user method
    // input_user Object
    // return void
    editUser(input_user)
    {      
      
      if(!this.validate(input_user)){
         return; 
      } 

      let data = {
        success: 'User successfully updated.',
        error: 'Update user unsuccessful.',
        options:{
          method: 'PUT',
          body: JSON.stringify(input_user),
          headers: {
              'Content-Type': 'application/json'
          }
        }
      }

      this.cudUser(data)    
      //close edit modal  
      this.closeModal('edit')
    },
    // Delete user method
    // key number
    // return void
    removeUser(key)
    {
      var confirm = window.confirm("Are you confirmed to delete this user?")
      if (confirm){
        this.deleteUser(key)
      }
    },
    // Call cud method to delete user
    // key number
    // return void
    deleteUser(key)
    {
      let content = {}
      content.id = key

      let data = {
        success: 'User successfully deleted.',
        error: 'Delete user unsuccessful.',
        options: {
          method: 'DELETE',
          body: JSON.stringify(content),
          headers: {
              'Content-Type': 'application/json'
          }
        }
      }
      this.cudUser(data)                  
    },
    // Display the modal depend on the post param
    // post Object
    // return void
    showModal(post){      
      if(post.modal == 'add'){
        this.showAddModel=true
      }else{
        //get the user data
        this.getOneUser(post.id)        
      }
    },
    // init validator, user object and close the modal
    // modal String
    // return void
    closeModal(modal)
    {
      this.validator = {
        name: [],            
        email: [],    
        phone: []
      }
      this.user = false
      if(modal == 'add'){
        this.showAddModel=false
      }else{
        this.showEditModel=false
      }
    },
    // Main validate function
    // user Object
    // return boolean
    validate(user){
      this.validator = {
          name: [],            
          email: [],    
          phone: []
        }
      this.checkRequired(user)           

      if(!user.name.match(/^[a-z\-'.\s]+$/ig)){
        this.validator['name']
        .push('Words, period, hypen, and single quote only');
      }

      if(!user.email.match(
        /^[a-z\-'.]+@[\w\d-]+\.[a-z]{2,3}(\.[a-z]{2,3})?$/g
      )){
        this.validator['email']
        .push('Invalid email');
      }

      if(!user.phone.match(
        /^\d{3}-\d{3}-\d{4}$/g
      )){
        this.validator['phone']
        .push('Invalid phone number');
      }

      for (let key in this.validator){
        if(this.validator[key].length > 0){
           return false; 
        } 
      }
      return true
    },
    // Validate fields are empty
    // user Object
    // return void
    checkRequired(user){
      for (let key in user){
          if(key == 'id') continue;
          if(!user[key]){            
              this.validator[key].push('This field cannot be empty');
          } 
      }
    }
  }
}
</script>

<style>
#app{
  font-family: 'Arial';
  background: linear-gradient(-45deg,  #93e0fc, #97ffe7);
}

main {
  padding: 40px
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-fade-enter-active {
  transition: all 0.8s ease-out;
}

.slide-fade-leave-active {  
  transition: all 0.8s ease-out;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}
</style>
