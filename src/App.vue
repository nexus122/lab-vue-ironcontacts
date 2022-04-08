<script>
import contacts from "./contacts.json";
export default {
  data() {
    return {
      allContacts: contacts,
      visibleContacts: contacts.slice(0, 5),
      orderCriteria: true
    };
  },
  methods:{
    addRandomContact(){
      let disponibles = this.allContacts.filter(contact => !this.visibleContacts.includes(contact))
      if(disponibles.length >= 1){
        this.visibleContacts.push(disponibles[Math.floor(Math.random()*disponibles.length)])
      }
    },
    sortByPopularity(){
      this.orderCriteria = true
    },
    sortByName(){
      this.orderCriteria = false
    },
    deleteContact(id){      
      let position = this.visibleContacts.findIndex(contact => contact.id == id);
      this.visibleContacts.splice(position, 1);
    }
  },
  computed:{
    visibleContactsSort(){
      // Se ordena por popularidad
      if(this.orderCriteria){
        return this.visibleContacts = this.visibleContacts.sort((c1,c2) => c1.popularity < c2.popularity ? 1: c1.popularity > c2.popularity ? -1:0)
      }else{
        return this.visibleContacts = this.visibleContacts.sort((c1,c2) => c1.name < c2.name ? 1: c1.name > c2.name ? -1:0)
      }
    }
  }
};
</script>

<template>

  <header class="container">
    <h1>Singu Contacts</h1>
    <div class="button-content">
      <button @click="addRandomContact()">Add Random Contact</button>
      <button @click="sortByPopularity()">Sort by Popularity</button>
      <button @click="sortByName()">Sort by Name</button>
    </div>
  </header>

  <table>
    <thead>
      <tr>
        <th>Picture</th>
        <th>Name</th>
        <th>Popularity</th>
        <th>Won Oscar</th>
        <th>Won Emy</th>
        <th>Delete</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="contact in visibleContactsSort" :key="contact.id">
        <td>
          <img :src="contact.pictureUrl" />
        </td>
        <td>{{ contact.name }}</td>
        <td>{{ contact.popularity.toFixed(2) }}</td>
        <td>
          <span class="premio" v-show="contact.wonOscar"
            ><i class="fa-solid fa-trophy"></i
          ></span>
        </td>
        <td>
          <span class="premio" v-show="contact.wonEmmy"
            ><i class="fa-solid fa-trophy"></i
          ></span>
        </td>
        <td>
          <button class="contrast" @click='deleteContact(contact.id)'> Delete </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style>
/* Estilos Generales */
img {
  max-width: 100px;
}
.premio i {
  font-size: 2em;
  color: black;
}

button {
    width: auto;
}

.button-content {
    display: flex;
    gap: 1em;
    flex-wrap: wrap;
}
</style>
