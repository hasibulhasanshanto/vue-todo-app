<template>
  <div class="container">
    <div class="add-item">
      <div class="add-icon" @click="addItem">
        <i class="fas fa-plus"></i>
      </div>

      <form @submit.prevent="addItem">
        <input type="text" placeholder="Add a To-do..." v-model="entry">
      </form>

      <div class="feature-icon" :class="{'red-bg': entryFavorite}">
        <i v-if="!entryFavorite" class="far fa-star" @click="entryFavorite = !entryFavorite"></i>
        <i v-else class="far fa-star" @click=" entryFavorite = !entryFavorite"></i>
      </div>
    </div> 

    <div class="todo-list">
      <div class="item" :class="{'show': item.show }" v-for="(item, index) in todoList" :key="index">
        <div class="item-checkbox">
          <i v-if="!item.complete" class="fas fa-square"  @click="completeItem(item)"></i>
          <i v-else class="fas fa-check-square"></i>
        </div>
        <div class="item-title">
          {{ item.title }}
        </div>
        <div class="feature-icon" :class="{'red-bg': item.favorite}">
          <i v-if="!item.favorite" class="far fa-star" @click="setFavoriteItem(item)"></i>
          <i v-else class="fas fa-star" @click="item.favorite = !item.favorite"></i>
        </div>
      </div>
    </div> 

    <div class="show-completed" v-if="completedTodoList.length > 0">
      <div class="button" @click="showCompletedList = !showCompletedList">
        <span v-if="!showCompletedList">Show</span><span v-else>Hide</span> completed todo's
      </div>
    </div> 

    <div class="todo-list complete-list" v-if="showCompletedList">
      <div class="item" :class="{'show': item.show }" v-for="(item, index) in completedTodoList" :key="index">
        <div class="item-checkbox">
          <i v-if="!item.complete" class="fas fa-check-square" @click="uncompleteItem(item)"></i>
          <i v-else class="fas fa-check-square" ></i>
        </div>
        <div class="item-title">
          <del>{{ item.title }}</del>
        </div>
        <div class="feature-icon" :class="{'red-bg': item.favorite}">
          <i v-if="!item.favorite" class="far fa-star"></i>
          <i v-else class="fas fa-star"></i>
        </div>
      </div>
    </div> 
  </div>
</template>

<script>

export default {
  data: () => {
    return {
      entry: '',
      entryFavorite: false,
      todoList: [],
      completedTodoList: [],
      showCompletedList : false

    }
  },

  methods: {
    addItem(){
      if (this.entry !== '') {
        let date = new Date;
        let newEntry = {
          id: date.getTime(),
          title: this.entry,
          favorite: this.entryFavorite,
          complete: false,
          show: false
        }
        if (newEntry.favorite) {
          //push first item in first position
          this.todoList.splice(0, 0, newEntry);
        } else{
           //push first item in last position
          this.todoList.push(newEntry);
        }

        setTimeout(() => {
          this.todoList.find(element => element.id === newEntry.id).show = true;
        }, 10);
      }
      this.entry = '';
      this.entryFavorite = false;

    },
    setFavoriteItem(item){
      item.favorite = !item.favorite;
      item.show = false;

      setTimeout(() => {
        let index = this.todoList.findIndex(element => element.id === item.id);
        this.todoList.splice(index, 1);
        this.todoList.splice(0, 0, item);
        item.show = true;

      }, 500);
    },
    completeItem(item){
      item.complte = !item.complete;
      item.show = false;

      setTimeout(() => { 
        this.completedTodoList.push(item);
        let index = this.todoList.findIndex(element => element.id === item.id);
        this.todoList.splice(index, 1); 
        item.show = true;

      }, 500);
    },
    uncompleteItem(item){
      item.complte = !item.complete;
      item.show = false;

      setTimeout(() => {
        if (item.favorite) {
          this.todoList.splice(0, 0, item);

        } 
        else{
          this.todoList.push(item);
        }

        let index = this.completedTodoList.findIndex(element=> element.id === item.id);
        this.completedTodoList.splice(index, 1);
        item.show = true;

      }, 500);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.12.1/css/all.min.css');


  .container{
    padding: 10px;
    width: calc(100% - 20px);
    min-height: calc(100% - 20px);

    @mixin feature-icon($color){
      grid-column-start: 3;
      grid-column-end: 3;
      display: flex;
      justify-content: center;
      align-items: center;
      color: $color;
      font-size: 1.6rem;
      margin:  0 15px;
      cursor: pointer;
    }


    .add-item{
      display: grid;
      grid-template-columns: 70px auto 70px;
      grid-template-rows: 60px;
      width: 100%;
      height: 60px;
      background: rgba($color: #000000, $alpha: .3);
      border-radius: 5px;
      overflow: hidden;
    }
    .add-icon{
      grid-column-start: 1;
      grid-column-end: 1;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #ffffff;
      font-size:  2rem;
      cursor: pointer;
    }

    input{
      grid-column-start: 2;
      grid-column-end: 2;
      background: none;
      border: none;
      width: 100%;
      height: 60px;
      color: #fff;
      font-size: 1.6rem;

      &::placeholder{
        color: #fff; 
      }
      &:focus{
        outline: none;
      }
    }

    .feature-icon{
      @include feature-icon(#fff);
    }
    .todo-list{
      padding-top: 20px;
      

      .item{
        display: grid;
        grid-template-columns: 70px auto 70px;
        grid-template-rows: 60px;
        width: 100%;
        height: 60px;
        margin-bottom: 2px;
        background: rgba($color: #E100FF, $alpha: .5);
        border-radius: 8px;
        overflow: hidden;
        opacity: 0;
        transition: all .5s linear;

        .item-checkbox{
          grid-column-start: 1;
          grid-column-end: 1;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: 1.6rem;
          color: #c8d6e5;
          cursor: pointer;
        }

        .item-title{
          grid-column-start: 2;
          grid-column-end: 2;
          display: flex;
          align-items: center;
          color: #c8d6e5;
          justify-content: flex-start;
          font-size: 1.6rem;
        }

        .feature-icon{
          @include feature-icon(#000);
        }

      }

      .show {
        opacity: 1;
      }
    }

    .complete-list {
      .item {
        position: relative;
        background: rgba($color: #7F00FF, $alpha: .5);

        &::before {
          content: '';
          position: absolute;
          top: 30px;
          width: calc(100% -120px);
          margin: 0 60px;
          // border-bottom: 2px solid #555;
        }
      }
    }

    .show-completed{
      display: flex;
      justify-content: center;
      align-items: center;
      padding-top: 20px;

      .button{
        color:#fff;
        padding: 10px;
        font-size: 1.3rem;
        text-transform: uppercase;
        background: rgba($color: #000000, $alpha: .3);
        border-radius: 5px;
        overflow: hidden;
        cursor: pointer;
      }
    }
    .red-bg{
      background-color: #c23741;
      color: #fff !important;
    }
  }
 
</style>
