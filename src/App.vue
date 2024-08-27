<script setup>
import { ref } from 'vue'
import SearchResults from './components/SearchResults.vue' 
import axios from "axios";

const domain = ref('')
const info = ref('domain')
const results = ref({
  domain_name: ''
})

function submit() {
  axios.get('http://127.0.0.1:8000/api/home', {
    params: {
      domain_name: domain.value
    },
  }).then((response) => {
    results.value = response.data;

  });
}

const truncateString = (string = '', maxLength = 50) => 
  string.length > maxLength 
    ? `${string.substring(0, maxLength)}…`
    : string

</script>

<template>
  <div class="col-lg-10 mx-auto p-4 py-md-5">
    <header class="d-flex align-items-center pb-3 mb-5 border-bottom">
      <span class="fs-4">TLV300 Exam</span>
    </header>
    <main>
      <div>
        <div class="input-group mb-3">
          <input type="text" class="form-control" placeholder="Enter domain name" aria-label="Domain Name" aria-describedby="search" v-model="domain">
          <button class="btn btn-outline-secondary" type="button" id="search" @click="submit">Search</button>
        </div>
        <SearchResults :domain="domain" />
        <br>
        <div class="form-check">
          <input class="form-check-input" type="radio" id="radDomain" v-model="info" value="domain">
          <label class="form-check-label" for="radDomain">
            Domain Information
          </label>
        </div>
        <div class="form-check">
          <input class="form-check-input" type="radio" id="radContact" v-model="info" value="contact">
          <label class="form-check-label" for="radContact">
            Contact Information
          </label>
        </div>
        <br>
        <br>

        <div v-if="info == 'domain'">
          <h2>Domain Information</h2>
          <table class="table">
            <thead>
              <tr>
                <th>Domain Name</th>
                <th>Registrar</th>
                <th>Registration Date</th>
                <th>Expiration Date</th>
                <th>Estimated Domain Age</th>
                <th>Hostnames</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>{{ results.domain_name }}</td>
                <td>{{ results.registrar_name }}</td>
                <td>{{ results.registration_date }}</td>
                <td>{{ results.exp_date }}</td>
                <td>{{ results.est_domain_age }}</td>
                <td>
                  <ul class="list-unstyled">
                    <li v-for="(hostname, index) in results.hostnames" :key="index">{{ truncateString(hostname) }}</li>
                  </ul>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-if="info == 'contact'">
          <h2>Contact Information</h2>
          <table class="table">
            <thead>
              <tr>
                <th>Registrant Name</th>
                <th>Technical Contact Name</th>
                <th>Administrative Contact Name</th>
                <th>Contact Email</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>{{ results.registrant_name }}</td>
                <td>{{ results.tech_contact_name }}</td>
                <td>{{ results.admin_contact_name }}</td>
                <td>{{ results.contact_email }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
