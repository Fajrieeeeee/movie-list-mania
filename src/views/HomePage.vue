<template>
    <Suspense>
      <template #default>
          <div style="padding: 0;">
            <main>
              <div class="bg-overlay">
                <h1>Welcome to Movie List Mania</h1>
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Numquam error eaque sit illo? Rerum, quisquam, eum accusamus tenetur delectus dicta aut ipsam id at ea debitis esse ad tempore quae dolor nobis fugiat. Ex dolorum exercitationem odit. Natus quo quibusdam saepe dolorem odio consequuntur?</p>
                <router-link to="/popular" active-class="active" class="cta-button">See the most popular movies</router-link>
              </div>
            </main>
            <Header title="Popular" navigation="See all" navigationHref="/popular" />
            <div class="movie-container" style="margin-top: 0;">
                <AsyncComp :movies="movies" />
            </div>
        </div>
      </template>
      <template #fallback>
        <div class="movie-container">
            <LoadingVue />
        </div>
      </template>
    </Suspense>
</template>

<script>
import { getMovieList } from "../libs/api";
import { scrollTop } from "../libs/scrollTop";
import LoadingVue from "../components/Loading.vue";
import { defineAsyncComponent } from "vue";
import Header from "../components/Header.vue";

const AsyncComp = defineAsyncComponent(() => import("../components/MovieList.vue"));
export default {
  data() {
    return {
      movies: [],
    };
  },
  methods: {
    async getPopularMovies() {
      const popularMovies = await getMovieList(1);
      this.movies = popularMovies;
    },
  },
  mounted() {
    this.getPopularMovies();
    scrollTop();
  },
  components: {
    AsyncComp,
    LoadingVue,
    Header,
    Header
}
};
</script>

<style scoped>
    main {
      background-image: url('../assets/bg-movie-image.jpg');
      background-size: cover;
      min-height: 80vh;
      color: white;
      position: relative;
    }
    
    .bg-overlay {
      text-align: left;
      padding: 3em;
      background-color: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      flex-direction: column;
      gap: 30px;
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      z-index: 2;
    }

    .bg-overlay p {
      line-height: 25px;
    }

    main h1 {
        font-size: 3rem;
    }
    

    .cta-button {
      width: 270px;
      border: 2px solid #1ca00b;
      color: white;
      padding: 1em 2em;
      background-color: #1ca00b;
      font-size: 1rem;
      border-radius: 20px;
      cursor: pointer;
      text-decoration: none;
      text-align: center;
      transition: .3s all;
    }

    .cta-button:hover {
      background-color: white;
      color: #1ca00b;
    }

    @media (max-width: 576px) {

        main {
          padding-top: 150px;
        }
        .bg-overlay {
          padding: 2em;
          gap: 45px;
        }
        .cta-button {
            width: 200px;
            padding: .5em 1em;
        }
    }
</style>