<template>
  <div class="home">
    <div class="feature-card">
      <img src="@/assets/img/movie.jpg" alt="Poster" class="feature-img" />
      <div class="detail">
        <h3>Find your movie!</h3>
        <p>
          Enter a title below and get all movies with it.
        </p>
      </div>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search" />
      <input type="submit" value="Search" />
    </form>

    <div v-if="!movies.length && loading" class="loader">
      <div class="lds-roller">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>

    <div v-if="errors" class="empty">
      <p>{{ errors }}</p>
    </div>

    <div v-if="movies.length && !loading" class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID + movie.Title">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" :alt="movie.Title + 'Poster'" />
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);
    const loading = ref(false);
    const errors = ref("");

    const SearchMovies = () => {
      if (search.value !== "") {
        loading.value = true;
        movies.value = [];

        fetch(`${process.env.VUE_APP_URL}/?apikey=${process.env.VUE_APP_API_KEY}&s=${search.value}`)
          .then(res => res.json())
          .then(data => {
            if (data.Response === "True") {
              errors.value = "";
              movies.value = data.Search;
              search.value = "";
              loading.value = false;
              return;
            }

            errors.value = data.Error;
            loading.value = false;
            search.value = "";
          });
      }
    };

    return {
      search,
      movies,
      loading,
      errors,
      SearchMovies,
    };
  },
};
</script>

<style lang="scss" scoped>
.home {
  .feature-card {
    position: relative;

    .feature-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #ffffff;
        margin-bottom: 16px;
      }

      p {
        color: #ffffff;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #ffffff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #ffffff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;
        cursor: pointer;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .empty {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 50px;

    p {
      color: #ffffff;
      font-size: 20px;
      font-weight: 600;
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #ffffff;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #aaaaaa;
            font-size: 14px;
          }

          h3 {
            color: #ffffff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }

  .loader {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 20px;

    .lds-roller {
      display: inline-block;
      position: relative;
      width: 80px;
      height: 80px;

      div {
        animation: lds-roller 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
        transform-origin: 40px 40px;

        &:after {
          content: " ";
          display: block;
          position: absolute;
          width: 7px;
          height: 7px;
          border-radius: 50%;
          background: #cef;
          margin: -4px 0 0 -4px;
        }

        &:nth-child(1) {
          animation-delay: -0.036s;
        }

        &:nth-child(1):after {
          top: 63px;
          left: 63px;
        }

        &:nth-child(2) {
          animation-delay: -0.072s;
        }

        &:nth-child(2):after {
          top: 68px;
          left: 56px;
        }

        &:nth-child(3) {
          animation-delay: -0.108s;
        }

        &:nth-child(3):after {
          top: 71px;
          left: 48px;
        }

        &:nth-child(4) {
          animation-delay: -0.144s;
        }

        &:nth-child(4):after {
          top: 72px;
          left: 40px;
        }

        &:nth-child(5) {
          animation-delay: -0.18s;
        }

        &:nth-child(5):after {
          top: 71px;
          left: 32px;
        }

        &:nth-child(6) {
          animation-delay: -0.216s;
        }

        &:nth-child(6):after {
          top: 68px;
          left: 24px;
        }

        &:nth-child(7) {
          animation-delay: -0.252s;
        }

        &:nth-child(7):after {
          top: 63px;
          left: 17px;
        }

        &:nth-child(8) {
          animation-delay: -0.288s;
        }

        &:nth-child(8):after {
          top: 56px;
          left: 12px;
        }
      }
    }
  }

  @keyframes lds-roller {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}
</style>
