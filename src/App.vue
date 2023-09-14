<template>
  <div id="app">
    <h1>City List</h1>
    <to-do-form @todo-added="addToDo"></to-do-form>
    <h2 id="list-summary" ref="listSummary" tabindex="-1">{{ listSummary }}</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in ToDoItems" :key="item._id">
        <to-do-item :label="item.name" :label1="item.code" :done="item.done" :id="item._id"
          @checkbox-changed="updateDoneStatus(item._id)" @item-deleted="deleteToDo(item._id)"
          @item-edited="editToDo(item._id, $event)">
        </to-do-item>
      </li>
    </ul>
  </div>
</template>

<script>
import ToDoItem from "./components/ToDoItem.vue";
import ToDoForm from "./components/ToDoForm.vue";
import uniqueId from "lodash.uniqueid";

export default {
  name: "app",
  components: {
    ToDoItem,
    ToDoForm,

  },
  data() {
    return {
      ToDoItems: [],

    }
  },

  created() {
    try {
      this.getCity();
    } catch (error) {
      console.error('Error in created hook:', error);
    }
  },


  methods: {

    async addToDo(toDoLabel) {
      console.log(toDoLabel);
      const city = await this.sendCity(toDoLabel)
      console.log(city);
      this.ToDoItems.push({
        id: uniqueId("todo-"),
        label: city.name,
        label1: city.code,
        done: false,
      });
      this.refreshPage()
    },
 
    async deleteToDo(toDoId) {
      console.log(toDoId);
      await this.deleteCity(toDoId)
      const itemIndex = this.ToDoItems.findIndex((item) => item._id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
      this.$refs.listSummary.focus();
      //this.refreshPage()
    },
    editToDo(toDoId, newLabel) {
      this.updateCity(toDoId,newLabel)
      const toDoToEdit = this.ToDoItems.find((item) => item._id === toDoId);
      toDoToEdit.label = newLabel.name;
      toDoToEdit.label1 = newLabel.code;
       this.refreshPage()
    },
    async getCity() {
      try {

        const response = await fetch('http://localhost:3000/api/city', {
          method: 'GET',
          // You can add headers here if needed
        });

        if (response.ok) {

          // Successful response, parse JSON data
          const responseData = await response.json();
          console.log(responseData.data);
         
          this.ToDoItems = responseData.data; // Update the cities data in your component

        } else {

          // Handle errors here
          console.error('Error:', response.statusText);
        }
      } catch (error) {
        // Handle network errors here
        console.error('Network error:', error);
      }
    },
    async sendCity(data) {
      try {

        const response = await fetch('http://localhost:3000/api/city', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(data),
          // You can add headers here if needed
        });
        console.log(response);
        if (response.ok) {

          // Successful response, parse JSON data
          const responseData = await response.json();


          return responseData.data; // Update the cities data in your component

        } else {

          // Handle errors here
          console.error('Error:', response.statusText);
        }
      } catch (error) {
        // Handle network errors here
        console.error('Network error:', error);
      }
    },


    async updateCity(id,data) {
      try {

        const response = await fetch(`http://localhost:3000/api/city/${id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(data),
          // You can add headers here if needed
        });
        console.log(response);
        if (response.ok) {

          // Successful response, parse JSON data
          const responseData = await response.json();


          return responseData.data; // Update the cities data in your component

        } else {

          // Handle errors here
          console.error('Error:', response.statusText);
        }
      } catch (error) {
        // Handle network errors here
        console.error('Network error:', error);
      }
    },
    async deleteCity(id) {
      try {
console.log(id);
        const response = await fetch(`http://localhost:3000/api/city/${id}`, {
          method: 'DELETE',
          headers: {
            'Content-Type': 'application/json',
          },
         
        });
        console.log(response.status);
        if (response.ok) {
          console.log(response.data);
          // Successful response, parse JSON data
          await response.json();


         // Update the cities data in your component

        } else {

          // Handle errors here
          console.error('Error:', response.statusText);
        }
      } catch (error) {
        // Handle network errors here
        console.error('Network error:', error);
      }
    },
    refreshPage() {
      window.location.reload();
    },


  },

  computed: {
    listSummary() {
      const numberFinishedItems = this.ToDoItems.filter(
        (item) => item.done
      ).length;
      return `${numberFinishedItems} out of ${this.ToDoItems.length} items completed`;
    },
  },
};
</script>

<style>
/* Global styles */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}

.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}

.btn__filter {
  border-color: lightgrey;
}

.btn__danger:focus {
  outline-color: #c82333;
}

.btn__primary {
  color: #fff;
  background-color: #000;
}

.btn-group {
  display: flex;
  justify-content: space-between;
}

.btn-group>* {
  flex: 1 1 auto;
}

.btn-group>*+* {
  margin-left: 0.8rem;
}

.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}

[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}

[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}

@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}

.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}

[class*="stack"]>* {
  margin-top: 0;
  margin-bottom: 0;
}

.stack-small>*+* {
  margin-top: 1.25rem;
}

.stack-large>*+* {
  margin-top: 2.5rem;
}

@media screen and (min-width: 550px) {
  .stack-small>*+* {
    margin-top: 1.4rem;
  }

  .stack-large>*+* {
    margin-top: 2.8rem;
  }
}

/* End global styles */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}

@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}

#app>* {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}

#app>form {
  max-width: 100%;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}
</style>