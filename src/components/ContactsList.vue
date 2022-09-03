<template>
<div>
    <button @click="getRandomElem">Add Random Contact</button>
    <button @click="sortByname">Sort Contacts by Name</button>
    <button @click="sortByPopularity">Sort Contacts by Popularity</button>
</div>
  <table v-if="fiveContacts.length" class="contacts-table">
    <tr>
        <th><h2>Picture</h2></th>
        <th><h2>Name</h2></th>
        <th><h2>Popularity</h2></th>
        <th><h2>Won Oscar</h2></th>
        <th><h2>Won Emi</h2></th>
        <th><h2>Actions</h2></th>
    </tr>
    <tr class="contact-list" v-for="contact in fiveContacts" :key="contact.id">
        <td>
            <img :src="contact.pictureUrl" />
        </td>
        <td>
            {{ contact.name }}
        </td>
        <td>
            {{ contact.popularity.toFixed(2) }}
        </td>
        <td>
            <img v-if="contact.wonOscar" src="../assets/trophy.png" />
        </td>
        <td>
            <img v-if="contact.wonEmmy" src="../assets/trophy.png" />
        </td>
        <td>
            <button @click="deleteContact(fiveContacts, contact)">Delete</button>
        </td>
    </tr>
  </table>
</template>

<script>
import contacts from "../contacts.json"
export default {
    name:'ContactsList',
    data() {
        return {
            contactsData: contacts,
            fiveContacts: [],
        }
    },
    created() {
        for (let i=0; i<5; i++) {
            this.fiveContacts.push(this.contactsData.shift())
        }
    },
    methods: {
        getRandomInt (min, max) {
            min = Math.ceil(min);
            max= Math.floor(max);
            return Math.floor(Math.random() * (max - min) + min);
        },
        getRandomElem() {
            if(this.contactsData.length) {
                const myRandomPos = this.getRandomInt(0, this.contactsData.length - 1)
                const myRandomElem = this.contactsData[myRandomPos];
                const elemIsAvailableToShow = this.fiveContacts.findIndex(elem => elem === myRandomElem);

                if(elemIsAvailableToShow === -1) {
                    this.fiveContacts.push(myRandomElem);
                    const elemToRemoveIndex = contacts.findIndex(elem => elem === myRandomElem);;
                    contacts.splice(elemToRemoveIndex, 1);
                }
            }
        },
        sortByname() {
            this.fiveContacts.sort((a,b) => (a.name < b.name) ? -1: 1);
        },
        sortByPopularity() {
            this.fiveContacts.sort((a,b) => (a.popularity < b.popularity) ? 1 : -1);
        },
        deleteContact(contactId) {
            const contactIndexFound = this.fiveContacts.indexOf(elem => elem.id === contactId);
                this.contactsData.unshift(this.fiveContacts.splice(contactIndexFound, 1)[0]);
            
        }
        },
    }
</script>

<style>
img {
    width: 180px;
    height: 250px;
}
td {
    font-size: larger;
    font-weight: bolder;
}
table {
    margin: auto;
}
</style>