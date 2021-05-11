<template>
  <div id="app">
    <div id="login-modal" v-if="modalVisible">
      <form action="">
        <input type="text" name="username" id="username">
        <input type="password" name="password" id="password">
        <span class="submit" @click="allowAccess">Login</span>
        <span class="cancel" @click="closeModal">Cancel</span>
      </form>
    </div>
    <div id="selection-container">
      <h1>Selected Courses</h1>
      <div class="card" v-for="(lesson, idx) in filteredListSelected" :key="idx">
          <div>
            <img v-bind:src="lesson.image"/><br>
            <button :index="idx" class="remove-button" @click="removeLesson">Remove</button>
          </div>
          <div class="details">
            <p class="lesson-name">{{ lesson.name }}</p>
            <p class="author"><span>Author:</span> {{ lesson.author }}</p>
            <p class="publish-date"><span>Published:</span> {{ lesson.publishDate }}</p>
            <p class="duration"><span>Duration:</span> {{ lesson.duration }}</p>
            <p class="description">{{ lesson.description }}</p>
          </div>
      </div>
    </div>
    <div id="search-container">
      <div class="search-wrapper">
        <input type="text" v-model="search" placeholder="Search title.."/>
        <label>Search title:</label><br />
        <button class="sort-filter" @click="sortDate">Sort By Date</button>
        <button class="sort-filter" @click="sortDuration">Sort By Duration</button>
      </div>
      <div class="wrapper">
        <div class="card" v-for="(lesson, idx) in filteredList" :key="idx">
            <div>
              <img v-bind:src="lesson.image"/><br>
              <button :index="lesson.index" class="add-button" @click="addLesson">Add</button>
            </div>
            <div class="details">
              <p class="lesson-name">{{ lesson.name }}</p>
              <p class="author"><span>Author:</span> {{ lesson.author }}</p>
              <p class="publish-date"><span>Published:</span> {{ lesson.publishDate }}</p>
              <p class="duration"><span>Duration:</span> {{ lesson.duration }}</p>
              <p class="description">{{ lesson.description }}</p>
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {    
    data() {
      return {
        search: '',
        isAllowed: true,
        // modalVisible: false,
        isSortDate: false,
        isSortDuration: false,
        selectedList: [],
        lessonList: [
          {
            "index": 0,
            "name": "React - basics",
            "description": "This course is going to take you through basics of React.",
            "author": "James White",
            "publishDate": "12/03/2019",
            "duration": "00:03:56",
            "image": "https://cdn.auth0.com/blog/react-js/react.png"
          },
          {
            "index": 1,
            "name": "Vue - learn vue in an hour",
            "description": "This course teaches you how to build a vue application in an hour.",
            "author": "Michael Brown",
            "publishDate": "17/10/2019",
            "duration": "00:00:59",
            "image": "https://vuejs.org/images/logo.png"
          },
          {
            "index": 2,
            "name": "CSS Animations",
            "description": "Learn how to animate anything in CSS",
            "author": "Alan Smith",
            "publishDate": "04/12/2018",
              "duration": "00:02:11",
            "image": "https://cdn.pixabay.com/photo/2017/08/05/11/16/logo-2582747_960_720.png"
          },
          {
            "index": 3,
            "name": "JS - Zero to hero",
            "description": "Everything you need to know in JS",
            "author": "Sarah Parker",
            "publishDate": "12/03/2019",
              "duration": "01:01:35",
            "image": "https://cdn.pixabay.com/photo/2015/04/23/17/41/javascript-736400_960_720.png"
          }
        ]
      }
    },
    methods: {
      sortDate() {
        this.isSortDate = true;
        this.isSortDuration = false;
      },
      sortDuration() {
        this.isSortDate = false;
        this.isSortDuration = true;
      },
      addLesson(event) {
        // NOTE: Couldn't get the modal and authentication to work

        // if(!this.isAllowed) {
        //   this.openModal();
        //   let isModalOpen = this.modalVisible;
        //   while(isModalOpen) {
        //     isModalOpen = this.modalVisible;
        //   }
        // }
        if(this.isAllowed) {
          let selectedIndex = +(event.target.attributes.index.value);
          if(!~this.selectedList.indexOf(selectedIndex)) {
            this.selectedList.push(selectedIndex);
          }                    
        }
      },
      removeLesson(event) {
        let selectedIndex = +(event.target.attributes.index.value);
        let foundIndex = this.selectedList.indexOf(selectedIndex);
        this.selectedList.splice(foundIndex, 1);
      },

      // Commented out functionality as I can't get the fake authentication to work

      // allowAccess() {
      //   this.isAllowed = true;
      //   this.modalVisible = false;
      //   return true;
      // },
      // openModal() {
      //   this.modalVisible = true;
      // },
      // closeModal() {
      //   this.modalVisible = false;
      //   return false;
      // }
    },
    computed: {
      filteredList() {
        let filterList = this.lessonList.filter(lesson => {
          return (lesson.name.toLowerCase().includes(this.search.toLowerCase()) && 
            (this.selectedList.indexOf(lesson.index) == -1))
        }).sort((a, b) => this.isSortDate ? 
          ((a.publishDate.split('/').reverse().join('') > b.publishDate.split('/').reverse().join('')) ? 1 : -1) : 
          this.isSortDuration ?
          ((a.duration > b.duration) ? 1 : -1) :
          0
        );
        return filterList;
      },
      filteredListSelected() {
        let filterList = this.lessonList.filter(lesson => {
          return (this.selectedList.indexOf(lesson.index) > -1)
        });
        return filterList;
      }
    }
  }
</script>

<style lang="less">

html, 
body {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin-top: 16px;
  margin-bottom: 16px;
  font-family: Arial, Helvetica, sans-serif;
}
div#app {
  display: table;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  .search-wrapper {
    position: relative;
    text-align: center;
    label {
      position: absolute;
      font-size: 12px;
      color: rgba(0,0,0,.50);
      top: 8px;
      left: 12px;
      z-index: -1;
      transition: .15s all ease-in-out;
    }
    input {
      padding: 4px 12px;
      color: rgba(0,0,0,.70);
      border: 1px solid rgba(0,0,0,.12);
      transition: .15s all ease-in-out;
      background: white;
      &:focus {
        outline: none;
        transform: scale(1.05);
        + label {
            font-size: 10px;
            transform: translateY(-24px) translateX(-12px);
          }
      }
      &::-webkit-input-placeholder {
        font-size: 12px;
        color: rgba(0,0,0,.50);
        font-weight: 100;
      }
    }
  }
  .wrapper {
    display: flex;
    max-width: 500px;
    flex-wrap: wrap;
    padding-top: 0;
  }
}

#search-container,
#selection-container {
  display: table-cell;
  width: 400px;
  height: 100%;
}

#selection-container {
  background-color: rgb(240, 240, 240);
  float: left;
  margin-right: 15px;
  > h1 {
    text-align: center;
  }
}
#search-container {
  background-color: rgb(255, 255, 255);
  float: right;
}

.card {
  box-shadow: rgba(0, 0, 0, 0.117647) 0px 1px 6px, rgba(0, 0, 0, 0.117647) 0px 1px 4px;
  width: 400px;
  max-width: 400px;
  margin: 12px;
  transition: .15s all ease-in-out;
  display: table-row;
  background-color: rgb(255, 255, 255);
  >div {
    display: table-cell;
    vertical-align: top;
    padding: 5px;
    >.add-button {
      width: 100px;
      background-color: rgb(0,0,240);
      border: 1px solid rgb(0,0,0);
      color: rgb(255,255,255);
      display: none;
    }
    >.remove-button {
      width: 100px;
      background-color: rgb(240,0,0);
      border: 1px solid rgb(0,0,0);
      color: rgb(255,255,255);
      display: none;
    }
  }
  &:hover > div > .add-button,
  &:hover > div > .remove-button {
    display: block;
  }
  img {
    height: 100px;
  }
  small {
    font-size: 18px;
    padding: 4px;
  }

  .details {
    >p {
      >span {
        font-size: 18px;
        font-weight: 700;
      }
    }
    .lesson-name {
      font-size: 24px;
      font-weight: 700;
    }
    .description {
      font-style: italic;
    }
  }
}

</style>
