<template>
  <div class="input-box">
    <label class="details">{{lable}}</label>
    <input :type="type" :placeholder="place" :name="name">
  </div>
</template>

<script>
  export default{
      name: "InputSpace",
      props: {
          lable: String,
          type: String,
          name: String,
          place: String,
          result: String
      }
  }
</script>
<style scoped>
  input{
    height: 45px;
    width: 100%;
    outline: none;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 16px;
    padding-left: 15px;
  }
  .input-box{
    margin-bottom: 15px;
    width: calc(100% / 2 - 20px);
  }
  .input-box lable{
    display: block;
    font-weight: 500;
    margin-bottom: 5px;
    width: calc(100% / 2 - 20px);
  }
  /* .flex{
      display: flex;
  }
  label {
        display: inline-block;
        width: 150px;
        text-align: right;
        margin-right: 30px;
        background: blue;
  }
  .form-control {
  margin: 20px 0;
  } */
  /* .form-control label {
  margin-left: 50px;
  }
  .form-control input {
  width: 100%;
  height: 40px;
  margin-left: 20px;
  padding: 3px 7px;
  font-size: 17px;
 }
 .details{
     display: flex;
 } */
</style>
