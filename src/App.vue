<template>
  <div class="container shadow  w-8/12 mt-3 px-2 py-3 border">
    <div class="">
      <h1 class="text-center mb-10 text-xl font-bold text-sky-600">Read Al-Quran</h1>
      <div class="flex justify-between px-5">
        <select @change="getPacificSurah" class=" p-4 border border-sky-600 focus:border-sky-600 outline-none rounded"
          name="" id="">
          <option v-for="surah in surahs " :value="surah.number"> {{ surah.number }} . {{ surah.name }} -
            {{ surah.englishName }}</option>
        </select>
        <h4 class="p-4 border border-sky-600 rounded">{{ currentSurah.englishNameTranslation }} -
          {{ currentSurah.numberOfAyahs }}</h4>
      </div>

      <div v-if="loading" class="absolute top-2/4 left-2/4 flex justify-center align-middle">
        <h1 class="flex text-5xl"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
            stroke-width="1.5" stroke="currentColor" class="w-20 animate-spin">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M4.5 12a7.5 7.5 0 0015 0m-15 0a7.5 7.5 0 1115 0m-15 0H3m16.5 0H21m-1.5 0H12m-8.457 3.077l1.41-.513m14.095-5.13l1.41-.513M5.106 17.785l1.15-.964m11.49-9.642l1.149-.964M7.501 19.795l.75-1.3m7.5-12.99l.75-1.3m-6.063 16.658l.26-1.477m2.605-14.772l.26-1.477m0 17.726l-.26-1.477M10.698 4.614l-.26-1.477M16.5 19.794l-.75-1.299M7.5 4.205L12 12m6.894 5.785l-1.149-.964M6.256 7.178l-1.15-.964m15.352 8.864l-1.41-.513M4.954 9.435l-1.41-.514M12.002 12l-3.75 6.495" />
          </svg>
        </h1>
      </div>

      <div v-else class="px-4 py-10">
        <p v-for="ayah in currentSurah.ayahs" style="font-family:  Lateef, serif;"
          class="text-3xl tracking-widest mb-8"><span class="">{{ ayah.numberInSurah }}</span>
          {{ ayah.text }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      surahs: [],
      currentSurah: [],
      loading: true
    }
  },
  mounted() {
    axios.get('https://api.alquran.cloud/v1/surah')
      .then(respons => {
        this.surahs = respons.data.data;
      })
    this.querySpecificSurah(1)

  },
  methods: {
    getPacificSurah(e) {
      this.querySpecificSurah(e.target.value);
    },

    querySpecificSurah(suraName) {
      axios.get('https://api.alquran.cloud/v1/surah/' + suraName)
        .then(respons => {
          this.currentSurah = respons.data.data;
          console.log(respons.data.data)
          this.loading = false
        })

    }
  }
}
</script>