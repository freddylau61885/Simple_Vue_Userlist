<template>
  <div class="sucessFlesh">    
    <div class="dialogContent">
      {{successMsg}}
      <button type="button" 
              class="btn-close"                             
              v-on:click="$emit('update:successMsg', false)">
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SuccessMessage',
  props: {
    successMsg:String
  }
}
</script>

<style scoped>
.sucessFlesh{
  background-color: rgba(153, 250, 250, 0.8);
  width: 100%;
  position: fixed;     
  z-index: 2;  
}

.sucessFlesh .btn-close{
  width: 5px;
  height: 5px;
  float: right;  
}

.dialogContent{
  text-align: center;
  font-size: 1.3em;  
  color: rgb(110, 110, 110);
  padding:15px;  
}
</style>