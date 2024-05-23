<script setup>
const props = defineProps({
  character: {
    type: Object,
    required: true
  }
});
const getStatusColor = (status) => {
  switch (status) {
    case 'Alive':
      return 'green';
    case 'Dead':
      return 'red';
    case 'unknown':
      return 'white';
    default:
      return 'gray';
  }
};
</script>

<template>
  <div class="card__main">
    <div class="card__image">
      <img :src="character.image" :alt="character.name">
    </div>
    <div class="card__text">
      <div class="card__text__item card__text__main-info">
        <h2>{{ character.name }}</h2>
        <div class="card__text__status">
          <div class="circle" :style="{ backgroundColor: getStatusColor(character.status) }"></div>
          <h3> {{ character.status }} - {{ character.species }}</h3>
        </div>
      </div>
      <div class="card__text__item card__text__location">
        <h3>Last known location:</h3>
        <h4>{{ character.location.name }}</h4>
      </div>
      <div class="card__text__item card__text__first-seen">
        <h3>First seen in:</h3>
        <h4>{{ character.origin.name }}</h4>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
