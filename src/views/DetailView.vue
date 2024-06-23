<script setup>
import { ref, onMounted, watch } from 'vue'
import axios from 'axios'
import { useRoute, useRouter } from 'vue-router'
import HeaderSection from '@/components/HeaderSection.vue'
import FooterSection from '@/components/FooterSection.vue'

const route = useRoute()
const router = useRouter()
const movieID = ref(route.params.movieID)
const movie = ref(null)
const characters = ref([])
const recommendations = ref([])
const activeTab = ref('teaser')
const apikey = 'b52737e89efeec142ce18cd254d41ba2'

const fetchMovieDetails = async () => {
  try {
    const response = await axios.get(
      `https://api.themoviedb.org/3/movie/${movieID.value}?api_key=${apikey}&language=ko-KR`
    )
    movie.value = response.data
    console.log('Movie Details:', response.data)
  } catch (error) {
    console.error(error)
  }
  try {
    const videoResponse = await axios.get(
      `https://api.themoviedb.org/3/movie/${movieID.value}/videos?api_key=${apikey}&language=ko-KR`
    )
    if (videoResponse.data.results.length > 0) {
      const teaserKey = videoResponse.data.results[0].key
      movie.value.teaserKey = teaserKey
    }
  } catch (error) {
    console.error(error)
  }
  try {
    const charactersResponse = await axios.get(
      `https://api.themoviedb.org/3/movie/${movieID.value}/credits?api_key=${apikey}`
    )
    characters.value = charactersResponse.data.cast
    console.log('Characters:', characters.value)
  } catch (error) {
    console.error(error)
  }
  try {
    const recommendationsResponse = await axios.get(
      `https://api.themoviedb.org/3/movie/${movieID.value}/recommendations?api_key=${apikey}&language=ko-KR`
    )
    recommendations.value = recommendationsResponse.data.results
    console.log('Recommendations:', recommendations.value)
  } catch (error) {
    console.error(error)
  }
}

const navigateToDetail = (id) => {
  router.push({ name: 'detail', params: { movieID: id } })
}

watch(
  () => route.params.movieID,
  () => {
    movieID.value = route.params.movieID
    fetchMovieDetails()
  }
)

onMounted(fetchMovieDetails)
</script>

<template>
  <HeaderSection />
  <div v-if="movie" class="detail_img">
    <img
      :src="'https://image.tmdb.org/t/p/w1280/' + movie.backdrop_path"
      :alt="movie.title"
      class="backdrop"
    />
    <div class="overlay"></div>
    <div class="container">
      <div class="movie-details">
        <img
          :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
          :alt="movie.title"
          class="poster"
        />
        <div class="details">
          <h2 class="title">{{ movie.title }}</h2>
          <p class="overview">{{ movie.overview }}</p>
          <p class="info">평점: {{ movie.vote_average.toFixed(1) }}</p>
          <p class="info">인기도: {{ movie.popularity.toFixed(2) }}</p>
          <p class="info">개봉일: {{ movie.release_date }}</p>
          <p class="info">장르: {{ movie.genres.map((genre) => genre.name).join(', ') }}</p>
        </div>
      </div>
    </div>
  </div>
  <div v-if="movie" class="tabs">
    <button @click="activeTab = 'teaser'" :class="{ active: activeTab === 'teaser' }">티저</button>
    <button @click="activeTab = 'characters'" :class="{ active: activeTab === 'characters' }">
      등장인물
    </button>
    <button @click="activeTab = 'videos'" :class="{ active: activeTab === 'videos' }">
      관련영화
    </button>
  </div>
  <div v-if="movie && activeTab === 'teaser'" class="teaser">
    <iframe
      width="1280"
      height="600"
      :src="'https://www.youtube.com/embed/' + movie.teaserKey"
      frameborder="0"
      allowfullscreen
    ></iframe>
  </div>
  <div v-if="activeTab === 'characters'" class="characters">
    <div class="container">
      <div class="character-grid">
        <div v-for="character in characters" :key="character.id" class="character-card">
          <div class="image-wrapper">
            <img
              v-if="character.profile_path"
              :src="'https://image.tmdb.org/t/p/w185/' + character.profile_path"
              :alt="character.name"
              class="character-img"
            />
            <div v-else class="no-image">none</div>
          </div>
          <p class="character-role">{{ character.character }}</p>
          <p class="character-name">{{ character.name }}</p>
        </div>
      </div>
    </div>
  </div>
  <div v-if="activeTab === 'videos'" class="videos">
    <div class="recommendation-grid">
      <div
        v-for="recommendation in recommendations"
        :key="recommendation.id"
        class="recommendation-card"
      >
        <div class="recommendation-wrapper">
          <img
            :src="'https://image.tmdb.org/t/p/w185/' + recommendation.poster_path"
            :alt="recommendation.title"
            class="recommendation-img"
          />
          <button class="detail-button" @click="navigateToDetail(recommendation.id)">
            상세보기
          </button>
        </div>
        <div class="recommendation-details">
          <p class="recommendation-title">{{ recommendation.title }}</p>
          <p class="recommendation-release-date">
            <span>개봉일</span>{{ recommendation.release_date }}
          </p>
        </div>
      </div>
    </div>
  </div>
  <FooterSection />
</template>

<style lang="scss">
.detail_img {
  position: relative;
  width: 100%;
  height: 600px;

  img.backdrop {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.1) 0%,
      rgba(0, 0, 0, 0.8) 40%,
      black 100%
    );
    z-index: 1;
  }
  .container {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 80%;
    border-radius: 8px;
    z-index: 2;
    color: var(--white);
  }
}
.movie-details {
  display: flex;
  align-items: flex-start;
  margin-bottom: 20px;
  align-items: center;
  display: flex;
  justify-content: center;
}
.poster {
  width: 360px;
  height: auto;
  border-radius: 8px;
  margin-right: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.details {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  margin-top: 190px; /* 포스터 끝에 맞추기 위해 마진 조정 */
  margin-left: 10px; /* 포스터와의 간격 유지 */
  max-height: 400px;
}
.title {
  font-size: 3rem;
  margin-bottom: 20px;
  font-weight: 500;
}
.overview {
  width: 800px;
  margin-bottom: 15px;
  line-height: 1.6;
  font-size: 1.3rem;
  max-height: 100px; /* 고정 높이를 설정합니다 */
  overflow-y: auto; /* 스크롤을 추가합니다 */
}
.info {
  margin-bottom: 8px;
  font-size: 1em;
  align-self: flex-start;
}
.tabs {
  display: flex;
  justify-content: center;
  margin-top: 40px;
  button {
    padding: 10px 20px;
    margin: 0 10px;
    border: none;
    background: none;
    color: var(--white);
    border: 1px solid var(--white);
    font-size: 1.2em;
    border-radius: 20px;
    cursor: pointer;

    &.active {
      background-color: var(--white);
      border: 1px solid var(--mainBg);
      color: var(--mainBg);
    }
  }
}
.teaser,
.characters,
.videos {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
  padding-top: 20px;
  width: 100%;
  margin-bottom: 40px;
}
.teaser {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 40px;
  padding-top: 40px;
  width: 100%;
}

.teaser iframe {
  max-width: 100%;
}
.character-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  justify-items: center;
  margin: 20px 200px;
}
.character-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 10px;
}
.image-wrapper {
  position: relative;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  background-color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 10px;
}
.character-img {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  object-fit: cover;
}

.character-role {
  font-size: 1.4rem;
  font-weight: bold;
  text-align: center;
}
.character-name {
  font-size: 0.8rem;
  color: gray;
  text-align: center;
}

.no-image {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 140px;
  height: 140px;
  border-radius: 50%;
  background-color: lightgray;
  color: gray;
  font-size: 1.5rem;
  font-weight: bold;
}

.recommendation-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
  justify-items: center;
  margin: 20px 0;
}
.recommendation-card {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 10px;
  background-color: var(--black);
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s;

  &:hover {
    transform: scale(1.02);
  }
}
.recommendation-wrapper {
  position: relative;
  width: 185px;
  height: 278px;
  border-radius: 10px 10px 0 0;
  overflow: hidden;
}
.recommendation-img {
  width: fit-content;
  height: fit-content;
  object-fit: cover;
  padding-top: 12px;
}
.recommendation-details {
  padding: 10px;
  text-align: center;
}
.recommendation-title {
  font-size: 1rem;
  font-weight: bold;
}
.recommendation-release-date {
  font-size: 0.8rem;
  color: gray;
  display: flex;
  text-align: center;
  align-items: center;
  justify-content: center;

  span {
    padding-right: 4px;
    color: aquamarine;
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

.recommendation-wrapper:hover .detail-button {
  display: block;
}

.recommendation-wrapper:hover .recommendation-img {
  opacity: 0.7;
}

@media (max-width: 1100px) {
  .movie-details {
    img {
      width: 300px;
    }

    .details {
      margin-top: 130px;
      .title {
        font-size: 2.4rem;
      }

      .overview {
        width: 700px;
        font-size: 1rem;
      }

      .info {
        font-size: 0.9rem;
      }
    }
  }

  .character-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    justify-items: center;
    margin: 20px 200px;
  }

  .recommendation-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    justify-items: center;
    margin: 20px 0;
  }
}

@media (max-width: 880px) {
  .movie-details {
    img {
      width: 260px;
    }

    .details {
      margin-top: 100px;
      .title {
        font-size: 2.2rem;
      }

      .overview {
        width: 500px;
        font-size: 0.9rem;
      }

      .info {
        font-size: 0.8rem;
      }
    }
  }

  .character-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    justify-items: center;
    margin: 20px 200px;
  }

  .recommendation-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    justify-items: center;
    margin: 20px 0;
  }
}
</style>
