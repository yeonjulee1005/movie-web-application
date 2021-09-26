<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <Nuxt-link class="button" :to="{name: 'index'}">Back</Nuxt-link>
    <div class="movie-info">
      <div class="movie-img">
        <img :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`" alt="" />
      </div>
      <div class="movie-content">
        <h1>제목: {{ movie.title }}</h1>
        <p class="movie-fact tagline">
          <span>요약:</span> "{{ movie.tagline }}"
        </p>
        <p class="movie-fact">
          <span>개봉일:</span>
          {{
            new Date(movie.release_date).toLocaleString('ko-kr', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>상영시간:</span> {{ movie.runtime }} 분
        </p>
        <p class="movie-fact">
          <span>수익:</span>
          {{
            movie.revenue.toLocaleString('ko-KR', {
              style: 'currency',
              currency: 'KRW',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>요약: </span>{{ movie.overview }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'single-movie',
  data() {
    return {
      movie: '',
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 600,

  head() {
    return {
      title: this.movie.title,
    }
  },

  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=738e21763835a097dae15a2f0f5b0df2&language=ko-KR`
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style lang="scss" scoped>

.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width:800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width:800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
      

        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }

      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }    
  }
}

</style>