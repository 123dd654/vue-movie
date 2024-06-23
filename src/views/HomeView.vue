<template>
  <HeaderSection />
  <main id="main" role="main">
    <div class="container">
      <section class="slide">
        <swiper
          class="swiper"
          :modules="modules"
          :pagination="false"
          :effect="'coverflow'"
          :grab-cursor="true"
          :centered-slides="true"
          :slides-per-view="2"
          :loop="true"
          :loop-additional-slides="3"
          :coverflow-effect="{
            rotate: 0,
            stretch: 0,
            depth: 100,
            modifier: 2,
            slideShadows: true
          }"
          :navigation="{
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          }"
          :autoplay="{
            delay: 3000,
            disableOnInteraction: false
          }"
        >
          <swiper-slide class="slide-content" v-for="movie in latestMovies" :key="movie.id">
            <section class="inner-slide">
              <div class="image-containerr">
                <img
                  :src="'https://image.tmdb.org/t/p/w1280' + movie.backdrop_path"
                  :alt="movie.title"
                  class="movie-poster"
                />
                <div class="overlay"></div>
              </div>
              <div class="slide-info">
                <h2>{{ movie.title }}</h2>
                <p>{{ movie.overview }}</p>
                <span><em>개봉일</em> {{ movie.release_date }}</span>
              </div>
              <button class="detail-buttonn" @click="navigateToDetail(movie.id)">
                상세보기
              </button>
            </section>
          </swiper-slide>
          <div class="swiper-button-next"></div>
          <div class="swiper-button-prev"></div>
        </swiper>
      </section>
      <div class="view__inner">
        <section class="view__card style1">
          <h3>New movies 🎬</h3>
          <swiper
            :spaceBetween="30"
            :slidesPerView="3"
            :centeredSlides="false"
            :loop="true"
            :pagination="{
              clickable: true
            }"
            :navigation="true"
            :modules="modules"
            class="mySwiper"
          >
            <swiper-slide v-for="movie in latestMovies" :key="movie.id">
              <div class="image-container">
                <img
                  :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
                  :alt="movie.title"
                  class="movie-poster"
                />
                <div class="overlay"></div>
                <button class="detail-button" @click="navigateToDetail(movie.id)">
                  상세보기
                </button>
              </div>
              <span>{{ movie.title }}</span><br />
              <span><em>개봉일</em>{{ movie.release_date }}</span>
            </swiper-slide>
          </swiper>
        </section>
        <section class="rank">
          <h3>Top Ranking 👑</h3>
          <p v-for="(movie, index) in TopMovies.slice(0, 5)" :key="movie.id">
            {{ index + 1 }}. <div class="title_img">
              <img :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path" alt="">
            </div><span>{{ movie.title }}</span><em>{{ movie.vote_average.toFixed(0) / 2 }}</em>
          </p>
        </section>
      </div>
      <section class="view_card_bottom1">
        <h3>Popular movies 🎬</h3>
        <swiper
          :spaceBetween="30"
          :slidesPerView="5"
          :centeredSlides="false"
          :loop="true"
          :pagination="{
            clickable: true
          }"
          :navigation="true"
          :modules="modules"
          class="mySwiper"
        >
          <swiper-slide v-for="movie in PopulartMovies" :key="movie.id">
            <div class="image-container">
              <img
                :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
                :alt="movie.title"
                class="movie-poster"
              />
              <div class="overlay"></div>
              <button class="detail-button" @click="navigateToDetail(movie.id)">
                  상세보기
                </button>
            </div>
            <span>{{ movie.title }}</span><br />
            <span><em>개봉일</em>{{ movie.release_date }}</span>
          </swiper-slide>
        </swiper>
      </section>
      <section class="view_card_bottom2">
        <h3>Top Rated movies 🎬</h3>
        <swiper
          :spaceBetween="30"
          :slidesPerView="5"
          :centeredSlides="false"
          :loop="true"
          :pagination="{
            clickable: true
          }"
          :navigation="true"
          :modules="modules"
          class="mySwiper"
        >
          <swiper-slide v-for="movie in TopMovies" :key="movie.id">
            <div class="image-container">
              <img
                :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
                :alt="movie.title"
                class="movie-poster"
              />
              <div class="overlay"></div>
              <button class="detail-button" @click="navigateToDetail(movie.id)">
                  상세보기
                </button>
            </div>
            <span>{{ movie.title }}</span><br />
            <span><em>개봉일</em>{{ movie.release_date }}</span>
          </swiper-slide>
        </swiper>
      </section>
      <section class="view_card_bottom3">
        <h3>Upcoming movies 🎬</h3>
        <swiper
          :spaceBetween="30"
          :slidesPerView="5"
          :centeredSlides="false"
          :loop="true"
          :pagination="{
            clickable: true
          }"
          :navigation="true"
          :modules="modules"
          class="mySwiper"
        >
          <swiper-slide v-for="movie in comingMovies" :key="movie.id">
            <div class="image-container">
              <img
                :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
                :alt="movie.title"
                class="movie-poster"
              />
              <div class="overlay"></div>
              <button class="detail-button" @click="navigateToDetail(movie.id)">
                  상세보기
                </button>
            </div>
            <span>{{ movie.title }}</span><br />
            <span><em>개봉일</em>{{ movie.release_date }}</span>
          </swiper-slide>
        </swiper>
      </section>
    </div>
  </main>
  <FooterSection />
</template>


<script>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/pagination'
import 'swiper/css/navigation'
import 'swiper/css/effect-coverflow'
import '../assets/scss/style.scss'
import { Autoplay, Pagination, Navigation, EffectCoverflow } from 'swiper/modules'
import HeaderSection from '@/components/HeaderSection.vue'
import FooterSection from '@/components/FooterSection.vue'
import { useRouter } from 'vue-router'

export default {
  components: {
    HeaderSection,
    FooterSection,
    Swiper,
    SwiperSlide
  },
  setup() {
    const latestMovies = ref([])
    const PopulartMovies = ref([])
    const TopMovies = ref([])
    const comingMovies = ref([])
    const apikey = 'b52737e89efeec142ce18cd254d41ba2'
    const router = useRouter()

    const fetchMovies = async () => {
      try {
        const latestResponse = await axios.get(
          `https://api.themoviedb.org/3/movie/now_playing?api_key=${apikey}&language=ko-KR&page=1`
        )
        latestMovies.value = latestResponse.data.results.slice(0, 10)
        console.log(latestResponse)
      } catch (error) {
        console.log(error)
      }

      try {
        const PopulartResponse = await axios.get(
          `https://api.themoviedb.org/3/movie/popular?api_key=${apikey}&language=ko-KR&page=1`
        )
        PopulartMovies.value = PopulartResponse.data.results.slice(0, 10)
      } catch (error) {
        console.log(error)
      }

      try {
        const TopResponse = await axios.get(
          `https://api.themoviedb.org/3/movie/top_rated?api_key=${apikey}&language=ko-KR&page=1`
        )
        TopMovies.value = TopResponse.data.results.slice(0, 10)
      } catch (error) {
        console.log(error)
      }

      try {
        const comingResponse = await axios.get(
          `https://api.themoviedb.org/3/movie/upcoming?api_key=${apikey}&language=ko-KR&page=1`
        )
        comingMovies.value = comingResponse.data.results.slice(0, 10)
      } catch (error) {
        console.log(error)
      }
    }

    const navigateToDetail = (movieId) => {
      router.push({ name: 'detail', params: { movieID: movieId } })
    }

    onMounted(fetchMovies)

    return {
      latestMovies,
      PopulartMovies,
      TopMovies,
      comingMovies,
      modules: [Autoplay, Pagination, Navigation, EffectCoverflow],
      navigateToDetail
    }
  }
}
</script>

<style scoped>
main{
  height: 3230px;
}

.slide {
  width: 100%;
  height: 500px;
  overflow: hidden;

  .swiper {
    height: 100%;
    width: 100%;
    padding-top: 50px;
    padding-bottom: 50px;

    .slide-content {
      width: fit-content;
      .inner-slide {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        width: 730px;
        height: 100%;
        position: relative;
        border-radius: 10px;
        overflow: hidden;

        .image-container {
          position: relative;
          width: 100%;
          height: 100%;

          .movie-poster {
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
            background: linear-gradient(to bottom, rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 0.8) 40%, black 100%);
            z-index: 1;
          }
        }

        .image-containerr{
          position: relative;
          width: 100%;
          height: 100%;

          .movie-poster {
            width: 100%;
            height: 100%;
            object-fit: cover;
            max-width: 100%;
          }

          .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, rgba(0, 0, 0, 0) 0%, rgba(0, 0, 0, 0.8) 50%, black 100%);
            z-index: 1;
          }

        }

        .slide-info {
          position: absolute;
          bottom: 20px;
          left: 20px;
          color: #fff;
          z-index: 2;

          h2 {
            margin: 0;
            font-size: 2rem;
            font-weight: 800;
          }

          p {
            width: 480px;
            overflow: hidden;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            text-overflow: ellipsis;
            margin-bottom: 20px;
          }

          span {
            font-size: 14px;
            color: #ccc;

            em {
              padding-right: 4px;
              font-size: 0.9rem;
              color: aquamarine;
            }
          }
        }

        .detail-buttonn {
          position: absolute;
          bottom: 20px;
          right: 20px;
          padding: 10px 20px;
          font-size: 16px;
          color: #fff;
          background-color: #16182f;
          border: none;
          border-radius: 5px;
          cursor: pointer;
          z-index: 3;
        }
      }
    }
  }
}

.view__inner {
  display: flex;
  gap: 30px;

  .view__card {
    margin-top: 50px;
    height: 620px;
    width: 70%;
    background-color: #16182f;
    padding: 30px;
    border-radius: 20px;
    position: relative;

    h3 {
      font-family: 'theJamsil';
      font-weight: 300;
      margin-bottom: 30px;
      font-size: 26px;
    }

    .swiper {
      height: calc(100% - 40px);
    }

    .swiper-wrapper {
      height: 100%;
    }

    .swiper-pagination {
      position: absolute;
      bottom: 26px;
      width: 100%;
      text-align: center;
    }

    .swiper-pagination-bullet-active {
      background: #fff;
    }

    .swiper-button-next,
    .swiper-button-prev {
      top: 50%;
      transform: translateY(-50%);
      color: #fff;
    }

    span {
      &:nth-child(2) {
        font-size: 1.3rem;
        line-height: 1.3;
        font-weight: 600;
      }

      &:nth-child(4) {
        border-radius: 10px;
        padding: 2px;
        font-size: 0.9rem;

        em {
          padding-right: 4px;
          font-size: 1.1rem;
          color: aquamarine;
        }
      }
    }
  }
}

.image-container {
  position: relative;
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

.image-container:hover .detail-button {
  display: block;
}


.image-container .movie-poster {
  display: block;
  width: 100%;
  height: 400px;
  object-fit: cover;
  border-radius: 10px;
  transition: opacity 0.3s ease;
}

.image-container:hover .movie-poster {
  opacity: 0.7;
}

.image-container:hover .overlay {
  opacity: 1;
}

.movie-poster {
  width: 100%;
  height: 400px;
  object-fit: cover;
  border-radius: 10px;
  transition: opacity 0.3s ease;
}

.detail-icon:hover + .movie-poster {
  opacity: 0.7;
}

.swiper-wrapper {
  border-radius: 10px;
  height: 100%;
}


.movie-poster {
  width: 100%;
  height: 400px;
  object-fit: cover;
  border-radius: 10px;
}

.swiper-wrapper {
  border-radius: 10px;
}

.rank {
  margin-top: 50px;
  width: 30%;
  background-color: #16182f;
  padding: 30px;
  border-radius: 20px;

  h3 {
    font-family: 'theJamsil';
    font-weight: 300;
    margin-bottom: 30px;
    font-size: 26px;
  }

  .title_img {
    width: 30px;
    height: 30px;
    position: relative;
    margin: 5px;

    img {
      border-radius: 100px;
      width: 30px;
      height: 30px;
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
  }

  p {
    font-size: 1.5rem;
    margin-top: 10px;
    font-weight: 500;
    margin-bottom: 30px;
    position: relative;
    display: flex;
    align-items: center;

    span {
      color: #fff;
      font-size: 1.2rem;
      margin-left: 4px;
    }

    em {
      color: #fff;
      font-size: 1.1rem;
      position: absolute;
      right: 0;
    }

    &:nth-child(2) {
      color: red;
    }

    &:nth-child(3) {
      color: orange;
    }

    &:nth-child(4) {
      color: yellow;
    }
  }
}

.view_card_bottom1,
.view_card_bottom2,
.view_card_bottom3 {
  width: 100%;
  background-color: #16182f;
  margin-top: 30px;
  border-radius: 20px;
  padding: 30px;
  position: relative;

  h3 {
    font-family: 'theJamsil';
    font-weight: 300;
    margin-bottom: 30px;
    font-size: 26px;
  }

  .swiper {
    height: 500px;
  }

  .swiper-wrapper {
    height: 100%;
  }

  .swiper-pagination {
    position: absolute;
    bottom: -2px;
    width: 100%;
    text-align: center;
  }

  .swiper-pagination-bullet-active {
    background: #fff;
  }

  .swiper-button-next,
  .swiper-button-prev {
    top: 50%;
    transform: translateY(-50%);
    color: #fff;
  }

  span {
    &:nth-child(2) {
      font-size: 1.3rem;
      line-height: 1.3;
      font-weight: 600;
    }

    &:nth-child(4) {
      border-radius: 10px;
      padding: 2px;
      font-size: 0.9rem;

      em {
        padding-right: 4px;
        font-size: 0.9rem;
        color: aquamarine;
      }
    }
  }
}

/* Change the next and previous buttons to white */
:deep(.swiper-button-next::after), 
:deep(.swiper-button-prev::after) {
  color: #fff;
}

/* Change the pagination bullets to white */
:deep(.swiper-pagination-bullet) {
  background: rgba(255, 255, 255, 0.3); /* Semi-transparent white */
}

:deep(.swiper-pagination-bullet-active) {
  background: #fff; /* Solid white */
}
@media (max-width: 900px) {
  .view__card {
    width: 100% !important;
  }

  .view_card_bottom1 .swiper-slide,
  .view_card_bottom2 .swiper-slide,
  .view_card_bottom3 .swiper-slide {
    width: calc(100% / 3 - 10px) !important; /* 슬라이드 너비 조정 */
  }

  .view_card_bottom1 .swiper-wrapper,
  .view_card_bottom2 .swiper-wrapper,
  .view_card_bottom3 .swiper-wrapper {
    display: flex !important;
    flex-wrap: wrap !important;
    gap: 10px !important;
  }

  .slide .swiper-slide {
    display: flex;
    justify-content: center;
    align-items: center;
    width: calc(100% / 3 - 10px);
  }

  .slide .swiper .inner-slide {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .slide .swiper .inner-slide .image-containerr {
    width: 100%;
    height: auto;
  }

  .slide .swiper .inner-slide .slide-info {
    font-size: 0.8em;
  }

  .slide-info {
    p {
      width: 340px !important;
    }
  }

  .rank {
    display: none;
    width: 0;
  }

  button {
    font-size: 0.7rem !important;
  }
}

@media (max-width: 680px) {
  .view__card {
    width: 100% !important;
  }

  .view_card_bottom1 .swiper-slide,
  .view_card_bottom2 .swiper-slide,
  .view_card_bottom3 .swiper-slide {
    width: calc(100% / 3 - 10px) !important; /* 슬라이드 너비 조정 */
  }

  .view_card_bottom1 .swiper-wrapper,
  .view_card_bottom2 .swiper-wrapper,
  .view_card_bottom3 .swiper-wrapper {
    display: flex !important;
    flex-wrap: wrap !important;
    gap: 10px !important;
  }

  .slide .swiper-slide {
    display: flex;
    justify-content: center;
    align-items: center;
    width: calc(100% / 3 - 10px);
  }

  .slide .swiper .inner-slide {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .slide .swiper .inner-slide .image-containerr {
    width: 100%;
    height: auto;
  }

  .slide .swiper .inner-slide .slide-info {
    font-size: 0.7em;
  }

  .slide-info {
    p {
      width: 200px !important;
    }
  }

  .rank {
    display: none;
    width: 0;
  }

  button {
    font-size: 0.5rem !important;
  }
}
</style>

