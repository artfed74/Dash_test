<template>
    <h1>Добавление записи</h1>
    <div class="form-container">
      <form @submit.prevent="submitCard">
        <label for="title">Название:</label>
        <input type="text" id="title" v-model="title">
        <br>
        <label for="description">Описание:</label>
        <textarea id="description" v-model="description"></textarea>
        <br>
        <button type="submit" class="send">Отправить</button>
        <button type="button" class="cancell" @click="goBack">Отмена</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'FormPage',
    data() {
      return {
        title: '',
        description: '',
      };
    },
    methods: {
        submitCard() {
      const newCard = {
        id: (this.$data.cards && this.$data.cards.length) ? this.$data.cards.length + 1 : 1,
        title: this.title,
        description: this.description
      };

      let existingCards = JSON.parse(localStorage.getItem('cards')) || [];

      existingCards.push(newCard);

      localStorage.setItem('cards', JSON.stringify(existingCards));

      this.$router.push('/');
    }
  }
  };
  </script>
  
  <style scoped>
  .form-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 400px;
  }
  
  form {
    width: 400px;
    height: 350px;
    padding: 20px;
    margin-top: 50px;
    background-color: #f4f4f4;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    box-shadow: 0 0 10px #6cd670;
     
  }
  
  h1 {
    text-align: center;
    margin-bottom: 20px;
  }
  
  label {
    font-weight: bold;
  }
  
  input,
  textarea {
    padding: 8px;
    margin-bottom: 10px;
    border-radius: 5px;
    font-size: 18px;
    border: 1px solid #ccc;
    box-sizing: border-box; 
  }
  textarea{
    height: 150px;
   
  }
  
  .send {
    background-color: #6cd670;
    border: 0;
    color: white;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s;
    
  }
  
  .send:hover {
    background-color: #5bca5f;
  }
  .cancell{
    margin-top: 30px;
    background-color: #ca2c2c;
    border: 0;
    color: white;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s;
  }
  .cancell:hover {
    background-color: #a00d0d;
  }
  </style>