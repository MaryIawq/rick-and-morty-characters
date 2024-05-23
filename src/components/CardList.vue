<script setup>
import CardComponent from "@/components/CardComponent.vue";
import { ref, onMounted } from 'vue';
import axios from 'axios';

const nameFilter = ref('');
const statusFilter = ref('');

const characters = ref([]);
const currentPage = ref(1);
const totalPages = ref(1);

const fetchCharacters = async (page, name = '', status = '') => {
  try {
    const response = await axios.get('https://rickandmortyapi.com/api/character', {
      params: {
        page,
        name,
        status
      }
    });
    characters.value = response.data.results;
    totalPages.value = response.data.info.pages;
  } catch (error) {
    console.error('error');
  }
};


onMounted(() => {
  fetchCharacters(currentPage.value, nameFilter.value, statusFilter.value);
});

const goToPage = (page) => {
  if (page > 0 && page <= totalPages.value) {
    currentPage.value = page;
    fetchCharacters(page, nameFilter.value, statusFilter.value);
  }
};
const applyFilters = () => {
  currentPage.value = 1;
  fetchCharacters(currentPage.value, nameFilter.value, statusFilter.value);
};
</script>

<template>
  <div class="main__container">
    <div class="filter-form">
      <input v-model="nameFilter" type="text" placeholder="Name">
      <select v-model="statusFilter">
        <option value="">All Statuses</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Apply</button>
    </div>
    <div class="pagination">
      <button @click="goToPage(currentPage - 1)" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="goToPage(currentPage + 1)" :disabled="currentPage === totalPages">Next</button>
    </div>
    <div class="card__list">
      <CardComponent v-for="character in characters" :key="character.id" :character="character"></CardComponent>
    </div>
  </div>
</template>
