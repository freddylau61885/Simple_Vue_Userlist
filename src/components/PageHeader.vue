<template>
  <div>
    <p>
      <img src="../assets/logo.png" alt="Logo" width="50" height="50">
      <strong>User List</strong>
    </p>
  </div>
</template>

<script>
export default {
  name: 'PageHeader' 
}
</script>

<style scoped>
  div{
    padding: 10px;
    height:70px;
    box-shadow: 0 3px 10px #444;
    background-color: #fff;
    width: 100%;
    position: fixed;    
    z-index: 1;
  }
  div img{
    margin-right: 10px;
  }
</style>