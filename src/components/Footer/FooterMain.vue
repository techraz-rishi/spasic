<template>
  <div
    class="container"
    style="background: var(--color-main);box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.596);"
  >
    <div class="row">
      <div class="col" style="padding:0">
        <div class="ft-content d-flex flew-row align-items-center">
          <!-- control -->
          <div class="control">
            <a class="d-flex flex-row align-items-end justify-content-start">
              <i class="fa fa-step-backward fa-lg" @click="$store.dispatch('backSong')"></i>
              <i
                v-if="!$store.getters.isPlaying"
                class="fa fa-play fa-lg"
                @click="$store.dispatch('play')"
              ></i>
              <i v-else class="fa fa-pause fa-lg" @click="$store.dispatch('pause')"></i>
              <i class="fa fa-step-forward fa-lg" @click="$store.dispatch('nextSong')"></i>
              <i
                id="hi"
                class="fa fa-repeat fa-lg"
                @click="$store.commit('changeLoop')"
                v-if="$store.getters.loop == 0"
              ></i>
              <i
                id="hi"
                class="fa fa-repeat-1 fa-lg loopAll"
                @click="$store.commit('changeLoop')"
                v-else-if="$store.getters.loop == 1"
              ></i>
              <i
                id="hi"
                class="fa fa-repeat fa-lg loopAll"
                @click="$store.commit('changeLoop')"
                v-else
              ></i>
              <i
                id="hi"
                :class="{
                    fa: true,
                    'fa-random': true,
                    'fa-lg': true,
                    loopAll: $store.getters.random
                  }"
                @click="$store.dispatch('changeShuffle')"
              ></i>
              <img v-lazy="$store.getters.currentSong.songImage" class="hi991" />
            </a>
          </div>

          <!-- Song Info -->
          <nav
            class="song-info"
            style="cursor:pointer; user-select:none"
            @click="$emit('changeExpandState')"
          >
            <p style="margin-bottom: 0; margin-top: 10px;">{{ $store.getters.currentSong.name }}</p>
            <p>{{ $store.getters.currentSong.artists }}</p>
          </nav>

          <!-- Menu -->
          <div class="menu-bot d-flex flex-row align-items-center justify-content-start">
            <ul class="d-flex flex-row align-items-start justify-content-start">
              <li style="position:relative" class="vl">
                <i id="hi" class="fa fa-volume-up fa-lg"></i>
                <input
                  id="hi"
                  type="range"
                  class="custom-range"
                  min="0"
                  max="1"
                  step="0.1"
                  v-model="volume"
                />
              </li>
              <li>
                <i
                  id="hi"
                  class="far fa-heart fa-lg"
                  v-if="!$store.state.music_store.currentSong.like"
                  @click.stop="$store.dispatch('likeSong')"
                ></i>
                <i
                  id="hi"
                  class="fas fa-heart fa-lg"
                  style="color:var(--color-hover)"
                  v-else
                  @click.stop="$store.dispatch('unlikeSong')"
                ></i>
              </li>
              <li>
                <i
                  class="fa fa-list-ul fa-lg"
                  @click="$emit('changeCurrentListState')"
                  :class="{ actived: showCurrentList }"
                ></i>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    showCurrentList: Boolean,
  },
  created() {
    this.vl = this.volume;
  },
  data() {
    return {
      vl: 0,
    };
  },
  computed: {
    volume: {
      get() {
        return this.$store.state.music_store.player.volume;
      },
      set(value) {
        this.vl = value;
        this.$store.state.music_store.player.volume = value;
      },
    },
  },
};
</script>
<style scoped>
.control img {
  width: 50px;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 230px;
  border-radius: 6px;
}
.actived {
  color: var(--color-hover);
}
.loopAll {
  color: var(--color-hover);
}
.custom-range {
  position: relative;
  width: 70px;
  margin-left: 5px;
  bottom: 5px;
  -moz-transform: translateY(7px);
  border-radius: 10px;
  background: var(--color5);
  z-index: 1000;
  display: none;
}
.vl:hover .custom-range {
  display: inline-block;
}
input[type="range"]::-webkit-slider-runnable-track {
  width: 100%;
  height: 3px;
  cursor: pointer;
  border-radius: 10px;
}
input[type="range"]::-webkit-slider-thumb {
  height: 10px;
  width: 10px;
  border-radius: 25px;
  background: var(--color-hover);
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -4px;
}
input[type="range"]::-moz-range-track {
  width: 100%;
  height: 3px;
  cursor: pointer;
  border-radius: 10px;
}
input[type="range"]::-moz-range-thumb {
  height: 10px;
  width: 10px;
  border-radius: 25px;
  background: var(--color-hover);
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -4px;
}
</style>