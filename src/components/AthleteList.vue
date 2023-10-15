<template>
    <div class="athlete-list__page">
      <div class="athlete-list__filters">
        <filter class="athlete-list__filter">
          <select class="athlete-list__select" v-model="selectedSport">
            <option value="">All Sports</option>
            <option v-for="sport in sportsList" :value="sport">{{ sport }}</option>
          </select>
        </filter>
        <filter class="athlete-list__filter">
          <select class="athlete-list__select" v-model="sortOrder">
            <option value="asc">A-Z</option>
            <option value="desc">Z-A</option>
          </select>
        </filter>
      </div>
      
      <div class="athlete-list__list">
          <div class="athlete-list__item" tabindex="0" v-for="athlete in displayedAthletes" :key="athlete.id">
          <div class="athlete-list__image">
            <img v-if="athlete.imageUrl" :src="athlete.imageUrl" :alt="athlete.name" />
            <img v-else src="/otto.png" alt="Otto the Orange">
          </div>
          <div class="athlete-list__details">
            <div class="athlete-list__name">
              {{ athlete.name }}
            </div>
            <div class="athlete-list__sport">
              {{ athlete.sport }}
            </div>
          </div>
          
          <div class="athlete-list__overlay">
              <span class="athlete-list__teaser">{{ athlete.shortDescription }}</span>
              <a :href="athlete.url" class="athlete-list__link">Read More</a>
            </div>
        </div>
      </div>
  
      <button class="load-more" @click="loadMore">Load More</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        athletes: [],
        selectedSport: '',
        sortOrder: 'asc',
        itemsPerPage: 4,
        loadedItems: 0,
      };
    },
    computed: {
    sportsList() {
      const sports = new Set();
      this.athletes.forEach(athlete => {
        sports.add(athlete.sport);
      });
      return Array.from(sports);
    },
    displayedAthletes() {
      // Filter and sort athletes based on selectedSport and sortOrder
      const filteredAthletes = this.athletes
        .filter(athlete => this.selectedSport === '' || athlete.sport.toLowerCase() === this.selectedSport.toLowerCase())
        .sort((a, b) => (this.sortOrder === 'asc' ? a.name.localeCompare(b.name) : b.name.localeCompare(a.name)));

      // Return the sliced array based on loadedItems and itemsPerPage
      return filteredAthletes.slice(0, this.loadedItems + this.itemsPerPage);
    },
  },
    methods: {
      async fetchAthletes() {
        try {
          const response = await fetch('https://msolo585.github.io/notablealumni/athletesData.json');
          const data = await response.json();
          this.athletes = data;
        } catch (error) {
          console.error('Error fetching athletes:', error);
        }
      },
      loadMore() {
        this.loadedItems += this.itemsPerPage;
      },
    },
    created() {
      this.fetchAthletes();
    },
  };
  </script>
  