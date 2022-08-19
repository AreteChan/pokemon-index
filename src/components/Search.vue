<template>
  <div id="search">
    <input type="text" placeholder="Press enter to search">
    <i class="fas fa-search"></i>
  </div>
</template>

<script>
export default {
  name: 'Search',
}
</script>

<style scoped>
  i {
    position: relative;
    right: 40px;
    font-size: 20px;
  }
  input {
    width: 300px;
    height: 50px;
    border-radius: 25px;
    border: 0;
    padding: 20px;
    font-size: 20px;
    outline: 0;
  }
  input:hover {
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.1);
  }
  input:focus {
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.1);
  }
  #search {
    width: fit-content;
    margin: 10px auto;
  }
</style>
