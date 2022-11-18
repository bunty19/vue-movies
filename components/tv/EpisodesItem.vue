<template>
  <div :class="$style.item">
    <div :class="$style.image">
      <div>
      <button
        v-if="episode.episode_number"
            :class="$style.play"
            type="button"
            aria-label="Play Trailer"
            @click="openModal">
            <!-- eslint-disable-next-line -->
            <svg xmlns="http://www.w3.org/2000/svg" width="55" height="55" viewBox="0 0 55 55"><circle cx="27.5" cy="27.5" r="26.75" fill="none" stroke="#fff" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"/><path fill="none" stroke="#fff" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M20.97 40.81L40.64 27.5 20.97 14.19v26.62z"/></svg>
          </button>
      <img
        v-if="poster"
        v-lazyload="poster"
        class="lazyload"
        :alt="episode.name"
        >
        
      <span v-else>
        <!-- eslint-disable-next-line -->
        <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill-rule="evenodd" clip-rule="evenodd" fill="#999"><path d="M24 22h-24v-20h24v20zm-1-19h-22v18h22v-18zm-1 16h-19l4-7.492 3 3.048 5.013-7.556 6.987 12zm-11.848-2.865l-2.91-2.956-2.574 4.821h15.593l-5.303-9.108-4.806 7.243zm-4.652-11.135c1.38 0 2.5 1.12 2.5 2.5s-1.12 2.5-2.5 2.5-2.5-1.12-2.5-2.5 1.12-2.5 2.5-2.5zm0 1c.828 0 1.5.672 1.5 1.5s-.672 1.5-1.5 1.5-1.5-.672-1.5-1.5.672-1.5 1.5-1.5z"/></svg>
      </span>
    </div>
  </div>

    <h2 :class="$style.name">
      <strong>E{{ episode.episode_number | numberWithDoubleDigits }}</strong> {{ episode.name }}
    </h2>

    <div :class="$style.overview">
      {{ episode.overview | truncate(300) }}
    </div>
    <button
              v-if="episode.episode_number"
              class="button button--icon"
              :class="$style.trailer"
              type="button"
              @click="openModal">
              <!-- eslint-disable-next-line -->
              <span class="icon"><svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="#fff"><path d="M3 22v-20l18 10-18 10z"/></svg></span>
              <span class="txt">Play Now</span>
            </button>
    <div
      v-if="episode.air_date"
      :class="$style.aired">
      {{ episode.air_date | fullDate }}
    </div>
    <Modal
      v-if="modalVisible"
      :data="epiembed"
      type="iframe"
      @close="closeModal" />
  </div>
  
</template>

<script>
import { apiImgUrl } from '~/api';
import Modal from '~/components/Modal';

export default {
  components: {
    Modal,
  },
  props: {
    episode: {
      type: Object,
      required: true,
    },
    activeSeason: {
      type: Number,
      required: true,
    },
  },
  data () {
    return {
      modalVisible: false,
    };
  },
  computed: {
    poster () {
      if (this.episode.still_path) {
        return `${apiImgUrl}/w400${this.episode.still_path}`;
      } else {
        return null;
      }
    },
    epiembed() {
            return [{
                src: `https://embed.smashystream.com/playere.php?tmdb=${this.$route.params.id}&season=${this.activeSeason}&episode=${this.episode.episode_number}`,
            }];
        },
    
  },
  methods: {
    openModal () {
      this.modalVisible = true;
    },

    closeModal () {
      this.modalVisible = false;
    },
  },
};
</script>

<style lang="scss" module>
@import '~/assets/css/utilities/_variables.scss';

.item {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0.4rem;
  margin-bottom: 2rem;

  @media (min-width: $breakpoint-xsmall) {
    width: 50%;
  }

  @media (min-width: $breakpoint-medium) {
    width: 33.3333333%;
  }

  @media (min-width: 1450px) {
    width: 25%;
  }

  @media (min-width: 2000px) {
    width: 20%;
  }

  @media (min-width: 3000px) {
    width: 16.6666667%;
  }
}

.image {
  position: relative;
  height: 0;
  padding-top: 56.25%;
  margin-bottom: 1.5rem;
  overflow: hidden;
  background-color: $secondary-color;

  img,
  span {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  span {
    display: flex;
    align-items: center;
    justify-content: center;
  }
}

.name {
  margin-bottom: 1rem;
  font-size: 1.6rem;
  letter-spacing: $letter-spacing;

  strong {
    color: $primary-color;
  }
}

.overview {
  flex: 1 0 auto;
  margin-bottom: 1rem;
  font-size: 1.3rem;
  color: $text-color;
}

.aired {
  font-size: 1.2rem;
  color: $text-color-grey;
  letter-spacing: $letter-spacing;

  @media (min-width: $breakpoint-large) {
    font-size: 1.4rem;
  }
}
.trailer {
  margin-top: 3rem;

  @media (max-width: $breakpoint-medium - 1) {
    display: none;
  }

  @media (min-width: 1650px) {
    font-size: 0.9vw;
  }
}

.play {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 1;
  padding: 0;
  margin: 0;
  background: none;
  transform: translate(-50%, -50%);

  @media (min-width: $breakpoint-medium) {
    display: none;
  }
}
</style>
