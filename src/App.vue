<template>
  <div class="md:mx-[650px] mx-3">
    <div class="my-16"></div>
    <div class="flex flex-col h-screen my-auto mx-0 items-center">
      <div class="p-6 bg-gray-100 rounded-lg w-1/2">
          <h2 class="text-center mb-6 italic">Q abis dengerin</h2>
            <div v-if="track" class="mb-2">
              <img class="mx-auto" :src="track.image[2]['#text']" :alt="track.album">
              <p class="text-center mt-6">{{ track.artist['#text'] }}</p>
              <p class="text-center text-xl font-bold">{{ track.name }}</p>
              <p class="text-center text-sm uppercase mt-3">{{ track.album['#text'] }}</p>
            </div>
          <div v-else>
            <p>No recent track played.</p>
          </div>
      </div>

      <div class="my-6"></div>

      <div class="bg-gray-100 rounded-lg w-1/2">
        <p class="pt-3 text-center text-sm">Spotify last track played, API by last.fm</p>
        <ul class="py-2 flex flex-col mx-10 items-center mb-3">
          <li><p class="font-bold mr-1 mb-3">Repository</p></li>
          <li><a class="bg-gray-500 text-white p-2 rounded-lg" href="https://github.com/abimanyudrmwn">Github</a></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      track: null,
      apiKey: import.meta.env.VITE_APP_LASTFM_API_KEY,
      username: import.meta.env.VITE_APP_LASTFM_USERNAME,
    };
  },
  mounted() {
    axios
      .get(`https://ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=${this.username}&api_key=${this.apiKey}&format=json&limit=1`)
      .then((response) => {
        if (response.data.recenttracks.track.length > 0) {
          this.track = response.data.recenttracks.track[0];
        }
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>
