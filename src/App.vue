<template>
  <h1>IronContacts</h1>
  
  <button @click="_addRandomContact">Add random contact</button>
  <button @click="_sortByPopularity">Sort by popularity</button>
  <button @click="_sortByName">Sort by name</button>

  <table>
    <tr>
      <th>Picture</th>
      <th>Name</th>
      <th>Popularity</th>
      <th>Won Oscar</th>
      <th>Won Emmy</th>
    </tr>
    <tr v-for="(contact, index) in contacts" :key="index">
      <td><img :src="contact.pictureUrl" :alt="`${contact.name} picture`" /></td>
      <td>{{ contact.name }}</td>
      <td>{{ contact.popularity }}</td>
      <td><span v-show="contact.wonOscar">🏆</span></td>
      <td><span v-show="contact.wonEmmy">🏆</span></td>
    </tr>
  </table>
</template>

<script>
import contacts from "./contacts.json";

export default {
  name: "App",
  data() {
    return {
      contacts: [],
      initialContacts: 5,
      sortBy: null
    };
  },
  methods: {
    _addRandomContact(){
      // Generate random index
      const randomIndex = Math.floor(Math.random()*contacts.length);

      // Check if the index is not in this.contacts already (index >= 5)
      if(randomIndex >= this.initialContacts){
        this.contacts.push(contacts[randomIndex]);
        console.log(contacts[randomIndex].name);
        if(this.sortBy === 'name'){
          this._sortByName();
        }
        if(this.sortBy === 'popularity'){
          this._sortByPopularity();
        }
      }
      // If index < 5, execute the function again
      else {
        this._addRandomContact();
      }
    },
    _sortByPopularity(){
      this.contacts.sort((a, b) => b.popularity - a.popularity);
      this.sortBy = 'popularity';
    },
    _sortByName(){
      this.contacts.sort((a, b) => a.name.localeCompare(b.name));
      this.sortBy = 'name';
    }
  },
  created() {
    // this._getContacts();
    this.contacts = contacts.slice(0, this.initialContacts);
  },
};
</script>

<style scoped>
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
