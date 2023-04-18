<template>
  <h1>IronContacts</h1>
  
  <button class="action" @click="_addRandomContact">Add random contact</button>
  <button class="action" @click="_sortByPopularity">Sort by popularity</button>
  <button class="action" @click="_sortByName">Sort by name</button>

  <table>
    <tr>
      <th>Picture</th>
      <th>Name</th>
      <th>Popularity</th>
      <th>Won<br>Oscar</th>
      <th>Won<br>Emmy</th>
      <th>Actions</th>
    </tr>
    <tr v-for="(contact, index) in renderedContacts" :key="index">
      <td><img :src="contact.pictureUrl" :alt="`${contact.name} picture`" /></td>
      <td>{{ contact.name }}</td>
      <td>{{ contact.popularity }}</td>
      <td><span v-show="contact.wonOscar">🏆</span></td>
      <td><span v-show="contact.wonEmmy">✨</span></td>
      <td><button @click="_deleteContact(contact)">Delete</button></td>
    </tr>
  </table>
</template>

<script>
import contacts from "./contacts.json";

export default {
  name: "App",
  data() {
    return {
      renderedContacts: [],
      initialContacts: 5,
      sortBy: null
    };
  },
  methods: {
    _addRandomContact(){
      const randomIndex = Math.floor(Math.random() * contacts.length);
      const randomContact = contacts[randomIndex];
      const contactExists = this.renderedContacts.find(contact => contact.name === randomContact.name);

      // Check if the randomContact is not in this.renderedContacts 
      if (!contactExists) {
        this.renderedContacts.push(randomContact);
        if(this.sortBy === 'name'){
          this._sortByName();
        }
        if(this.sortBy === 'popularity'){
          this._sortByPopularity();
        }
      }
      // If randomContact is in this.renderedContacts, execute the function again
      else {
        this._addRandomContact();
      }
    },
    _sortByPopularity(){
      this.renderedContacts.sort((a, b) => b.popularity - a.popularity);
      this.sortBy = 'popularity';
    },
    _sortByName(){
      this.renderedContacts.sort((a, b) => a.name.localeCompare(b.name));
      this.sortBy = 'name';
    },
    _getContact(){

    },
    _deleteContact(contactToDelete){
      const indexToDelete = this.renderedContacts.findIndex(contact => contact.name === contactToDelete.name);

      if (indexToDelete !== -1) {
        this.renderedContacts.splice(indexToDelete, 1);
      }
    }
  },
  created() {
    this.renderedContacts = contacts.slice(0, this.initialContacts);
  },
};
</script>

<style scoped>
  button.action{
    margin: 0 5px;;
  }

  table{
    text-align: center;
  }

  table th{
    font-weight: 700;
  }

  table img{
    width: 100px;
  }
  
</style>
