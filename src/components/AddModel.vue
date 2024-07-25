<template>
  <div class="modal fade show d-block" 
       id="userModal" 
       tabindex="-1" 
       aria-labelledby="userModalLabel" 
       aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="userModalLabel">{{title}}</h5>
          <button type="button" 
                  class="btn-close" 
                  data-bs-dismiss="modal" 
                  aria-label="Close"
                  v-on:click="$emit('closemodal', 'add')"></button>
        </div>
        <div class="modal-body">
          <p>
            <strong>Name</strong> 
            <span><strong>{{validator.name[0]}}</strong></span>
            <br>
            <input type="text" 
                   v-model="input_user.name" 
                   placeholder="Full Name"
                   class="form-control"/>            
          </p>          
          <p>
            <strong>Email</strong> 
            <span><strong>{{validator.email[0]}}</strong></span>
            <br>
            <input type="text" 
                   v-model="input_user.email" 
                   placeholder="Email"
                   class="form-control"/>            
          </p>
          <p>
            <strong>Phone</strong> 
            <span><strong>{{validator.phone[0]}}</strong></span>
            <br>
            <input type="text" 
                   v-model="input_user.phone" 
                   placeholder="e.g. 204-321-1256"
                   class="form-control"/>            
          </p>
        </div>
        <div class="modal-footer">
          <button type="button" 
                  class="btn btn-secondary" 
                  data-bs-dismiss="modal"
                  v-on:click="$emit('closemodal', 'add')">Close</button>
          <button type="button" 
                  class="btn btn-primary"
                  data-bs-dismiss="modal"
                  v-on:click="$emit('submituser',input_user)">Submit</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddModel',
  props: {
    validator:Object
  },
  data() {
    return {  
      title: 'Add User',      
      input_user: {
        id: '',            
        name: '',            
        email: '',    
        phone: ''    
      }
    }
  }
}
</script>

<style>
#userModal{
  background-color: rgba(0,0,0,0.4);  
}

.modal-body p span{
  margin-left: 10px;
  color: rgb(216, 35, 35)
}

#userModal .modal-dialog .modal-header{
  color: #fff;
  text-shadow: 2px 2px 8px #000;
  background-image: url('../assets/modal_header.jpg');
  border-radius:0 ;
  border: none
}

#userModal .modal-dialog .modal-content{
  border-radius:0 ;
  border: 1px solid #000;
}

#userModal .modal-dialog .modal-content input{
  margin-top: 5px;
  border-radius:0 ;
  border: none;
  border-bottom: 1px solid #000 ;
  background-color: rgba(245, 244, 244, 0.6);
}

#userModal .modal-dialog .modal-footer{
  border-radius:0 ;
  background-color: rgb(218, 218, 218);
}
</style>