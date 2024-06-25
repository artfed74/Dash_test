<template>
    <div>
        <div class="container">
        <router-link to="/form" class="add-link">
            <button class="add-button">Добавить</button>
        </router-link>
        <label class="switch">
            <input type="checkbox">
            <span class="slider"></span>
        </label>
        <div class="text">
            Click&drag
        </div>
</div>
      <div
        class="card-container"
        @dragover.prevent
        @drop="handleDrop"
      >
      <div
      v-for="card in filteredCards"
      :key="card.id"
      class="card"
      draggable="true"
      @dragstart="handleDragStart(card)"
      @dragover="handleDragOver"
      @dragend="handleDragEnd"
    >
      <h3>{{ card.title }}</h3>
      <p>{{ card.description }}</p>
    </div>
      </div>
    </div>
  </template>
  
  <script>
  // значения из json файла
  import jsonData from '../../data.json';
  
  export default {
    name: "HomePage",
    data() {
      return {
        cards: jsonData
      };
    },
    mounted() {
      const localCards = JSON.parse(localStorage.getItem("cards"));
      if (localCards) {
        this.cards = localCards;
      }
    },
    computed: {
    filteredCards() {
    
      return this.cards.filter(card => card !== null);
    }
  },
    methods: {
      handleDragStart(card) {
        this.draggedCard = card;
      },
      handleDragOver(e) {
        e.preventDefault();
      },
      handleDrop(e) {
        if (!this.draggedCard) {
          return;
        }
        const indexToInsert = Array.from(e.target.parentElement.children).indexOf(e.target);
        const draggedIndex = this.cards.indexOf(this.draggedCard);
        this.cards.splice(draggedIndex, 1);
        this.cards.splice(indexToInsert, 0, this.draggedCard);
        this.saveCardsToLocalStorage();
        this.draggedCard = null;
      },
      handleDragEnd() {
        this.saveCardsToLocalStorage();
      },
      saveCardsToLocalStorage() {
        localStorage.setItem("cards", JSON.stringify(this.cards));
      }
    }
  };
  </script>
  
  <style scoped>
  .add-button {
    background-color: #6cd670;
    border: 0;
    color: white;
    width: 100px;
    height: 40px;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .add-button:hover {
    background-color: #5bca5f;
  }
  
  .card-container {
    margin-top: 30px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
  }
  
  .card {
    background-color: #f4f4f4;
    padding: 10px;
    border-radius: 5px;
    cursor: grab;
    transition: all 0.3s;
  }
  
  .card:hover {
    box-shadow: 0 0 5px #6cd670;
  }
  .checkbox-container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
}
input [type='checkbox']{
  
  position: absolute;
  opacity: 0;
  cursor: pointer;
}
.container {
  display: flex;
  align-items: center;
}

.add-link {
  text-decoration: none; 
  margin-right: 10px; 
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {
  display: none;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: #6cd670;
}

input:focus + .slider {
  box-shadow: 0 0 1px #6cd670;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
.text{
    font-size: 18px;
    margin-left: 15px;
}
  </style>