<template>
  <div class="errorFlesh">    
    <div class="dialogContent">
      {{errorMsg}}
      <button type="button" 
              class="btn-close"                             
              v-on:click="$emit('update:errorMsg', false)">
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ErrorMessage',
  props: {
    errorMsg:String
  }
}
</script>

<style scoped>
.errorFlesh{
  background-color: rgba(250, 153, 153, 0.8);
  width: 100%;
  position: fixed;
  z-index: 2;  
}

.errorFlesh .btn-close{
  width: 5px;
  height: 5px;
  float: right;  
}

.dialogContent{
  text-align: center;
  font-size: 1.2em;
  color: rgb(110, 110, 110);
  padding:15px;
}
</style>