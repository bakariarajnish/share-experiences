<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="fetchExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="showLoading">Loading...</p>
      <p v-else-if="!showLoading && error">{{ error }}</p>
      <p v-else-if="!showLoading && results.length === 0">
        Sorry, no data available for user experiences....
      </p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  //props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      showLoading: false,
      error: null,
    };
  },
  methods: {
    fetchExperiences() {
      this.showLoading = true;
      this.error = null;
      fetch(
        'https://http-request-project-697a2-default-rtdb.firebaseio.com/survey.json'
      )
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          const expResults = [];
          for (let id in data) {
            expResults.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = expResults;
          this.showLoading = false;
        })
        .catch((error) => {
          console.log(error);
          this.showLoading = false;
          this.error = 'Failed to fetch data. Please try in sometime.';
        });
    },
  },
  mounted() {
    this.fetchExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
