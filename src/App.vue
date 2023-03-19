<template>
  <div class="contacts">
    <h1>Contacts</h1>
    <div class="search-bar">
      <input
          type="text"
          placeholder="Search contacts"
          v-model="searchText"
      />
    </div>
    <div class="contacts-list">
      <div
          class="contact-card"
          v-for="(contact, index) in filteredContacts"
          :key="index"
      >
        <div class="contact-avatar">
          <img :src="getAvatar(contact.name)" alt="Avatar"/>
        </div>
        <div class="contact-details">
          <h2>{{ contact.name }}</h2>
          <p>{{ contact.email }}</p>
          <p>{{ contact.phone }}</p>
          <p><strong>Website:</strong> {{ contact.website }}</p>
          <p><strong>Company:</strong> {{ contact.company.name }} - {{ contact.company.catchPhrase }}</p>
          <p><strong>Address:</strong> {{ contact.address.street }}, {{ contact.address.suite }},
            {{ contact.address.city }}, {{ contact.address.zipcode }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Contacts',
  data() {
    return {
      contacts: [],
      searchText: '',
      filteredContacts: []
    };
  },
  mounted() {
    axios.get('https://jsonplaceholder.typicode.com/users')
        .then(response => {
          this.contacts = response.data;
          this.filteredContacts = response.data;
        })
        .catch(error => {
          console.log(error);
        });
  },
  methods: {
    getAvatar(name) {
      const initials = name.match(/\b\w/g) || [];
      return `https://ui-avatars.com/api/?name=${initials.join('')}`;
    }
  },
  watch: {
    searchText(newVal) {
      this.filteredContacts = this.contacts.filter(contact => {
        return (
            contact.name.toLowerCase().includes(newVal.toLowerCase()) ||
            contact.email.toLowerCase().includes(newVal.toLowerCase()) ||
            contact.phone.toLowerCase().includes(newVal.toLowerCase())
        );
      });
    }
  }
};
</script>

<style scoped>
.contacts {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.search-bar {
  margin-bottom: 2rem;
}

.contact-card {
  display: flex;
  margin-bottom: 1rem;
  padding: 1rem;
  border-radius: 0.5rem;
  background-color: #fff;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.1);
}

.contact-avatar {
  margin-right: 1rem;
  width: 64px;
  height: 64px;
  border-radius: 50%;
  overflow: hidden;
}

.contact-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.contact-details {
  flex-grow: 1;
}

h1 {
  margin-bottom: 2rem;
  font-size: 2rem;
  font-weight: bold;
}

h2 {
  margin-bottom: 0.5rem;
  font-size: 1.5rem;
  font-weight: bold;
}

p {
  margin: 0.5rem 0;
}
</style>