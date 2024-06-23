<template>
  <HeaderSection />
  <div class="container">
    <div>
      <h1>"{{ $route.query.q }}" 검색결과입니다.</h1>
      <div v-if="loading" class="loading">Loading...</div>
      <div v-else class="results">
        <div v-if="movies.length === 0" class="no-results">No results found</div>
        <div v-else class="grid">
          <div v-for="movie in movies" :key="movie.id" class="movie">
            <div class="movie-card">
              <div class="poster-wrapper">
                <img
                  :src="getImageUrl(movie.poster_path)"
                  alt="Movie Poster"
                  class="movie-poster recommendation-img"
                />
                <button class="detail-button" @click="viewDetails(movie.id)">상세보기</button>
              </div>
              <div class="movie-info">
                <h2 class="movie-title">{{ movie.title }}</h2>
                <p class="movie-release-date"><span>개봉일</span>{{ movie.release_date }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <FooterSection />
</template>

<script>
import axios from 'axios'
import HeaderSection from '@/components/HeaderSection.vue'
import FooterSection from '@/components/FooterSection.vue'

export default {
  data() {
    return {
      movies: [],
      loading: true
    }
  },
  created() {
    this.fetchMovies()
  },
  watch: {
    '$route.query.q'() {
      this.fetchMovies()
    }
  },
  methods: {
    async fetchMovies() {
      this.loading = true
      try {
        const response = await axios.get(
          `https://api.themoviedb.org/3/search/movie?api_key=b52737e89efeec142ce18cd254d41ba2&query=${this.$route.query.q}&page=1`
        )
        this.movies = response.data.results
      } catch (error) {
        console.error('Error fetching data:', error)
      } finally {
        this.loading = false
      }
    },
    viewDetails(movieID) {
      this.$router.push({ name: 'detail', params: { movieID: movieID } })
    },
    getImageUrl(posterPath) {
      return posterPath
        ? `https://image.tmdb.org/t/p/w500${posterPath}`
        : 'https://via.placeholder.com/300x450?text=No+Image'
    }
  },
  components: { HeaderSection, FooterSection }
}
</script>

<style scoped>
h1 {
  text-align: center;
  padding: 30px;
  font-weight: 900;
  font-size: 2.2rem;
}
.loading {
  text-align: center;
  margin: 20px;
}
.no-results {
  text-align: center;
  margin: 20px;
  font-size: 1.2em;
}
.results {
  min-height: 100vh;
  gap: 10px;
  margin-bottom: 50px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
  justify-items: center;
}

.movie {
  text-align: center;
}

.movie-card {
  background-color: var(--black);
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s;
  padding: 10px;
  position: relative;
}

.movie-card:hover {
  transform: scale(1.02);
}

.poster-wrapper {
  position: relative;
  overflow: hidden;
}

.movie-poster {
  width: 200px;
  height: 280px;
  border-bottom: 1px solid #333;
  transition: opacity 0.3s ease;

  img {
    width: 200px;
  }
}

.detail-button {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 10px 20px;
  font-size: 16px;
  color: #fff;
  background-color: #16182f;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  z-index: 3;
  display: none;
}

.poster-wrapper:hover .detail-button {
  display: block;
}

.poster-wrapper:hover .movie-poster {
  opacity: 0.7;
}

.movie-info {
  padding: 10px;
}

.movie-title {
  width: 200px;
  cursor: pointer;
  color: #fff;
  font-size: 1.1rem;
  /* margin: 10px 0; */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

span {
  padding-right: 4px;
  color: aquamarine;
}

.movie-release-date {
  color: #aaa;
  font-size: 0.9rem;
}

@media (max-width: 1220px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .grid {
    grid-template-columns: 1fr;
  }
}
</style>
