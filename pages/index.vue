<template>
  <div class="min-h-screen bg-neutral-200">
    <h3
      data-aos="fade-up"
      data-aos-duration="1000"
      class="text-3xl text-center text-neutral-700 font-bold pt-10"
    >
      Text analysis
    </h3>

    <p
      data-aos="fade-up"
      data-aos-duration="1000"
      class="text-center text-neutral-600 font-bold pt-10"
    >
      Let's analyze your text
    </p>

    <div
      data-aos="fade-up"
      data-aos-duration="1000"
      class="flex justify-center pt-10 flex-col gap-y-2 items-center"
    >
      <div class="sm:w-1/2 w-full m-1" v-if="!isLoading">
        <textarea
          @keyup.enter="analyzeText"
          v-model="text"
          :class="isLoading ? 'bg-gray-200 cursor-not-allowed' : 'bg-white'"
          class="w-full h-64 px-3 py-2 text-base text-gray-700 placeholder-gray-600 border rounded-lg focus:shadow-outline focus:outline-none"
          placeholder="Let's analyze your text here
     "
        ></textarea>
      </div>
      <div v-if="isLoading" class="sm:w-1/2 w-full">
        <!-- loader -->
        <div class="flex justify-center items-center">
          <div
            class="animate-spin rounded-full h-32 w-32 border-t-2 border-b-2 border-neutral-500"
          ></div>
        </div>
      </div>
      <div data-aos="fade-up" data-aos-duration="1000" class="flex justify-center">
        <div class="sm:w-1/2 w-full flex justify-center items-center mx-auto">
          <button
            class="px-4 py-2 font-bold text-white bg-neutral-500 justify-center flex items-center justify-self-center rounded hover:bg-neutral-700"
            @click="analyzeText"
            :disabled="isLoading"
          >
            {{ isLoading ? "Analyzing..." : "Analyze" }}
          </button>
        </div>
      </div>
    </div>

    <div
      v-if="!isLoading && theResponse"
      data-aos="fade-up"
      data-aos-duration="1000"
      class="flex justify-center flex-col gap-y-2 items-center py-5"
    >
      <!-- theResponse -->
      <div class="sm:w-1/2 w-full my-2">
        <div class="container mx-auto p-4 bg-white rounded" v-if="theResponse.length > 0">
          <div class="mb-4 flex justify-center flex-col items-center gap-y-2">
            <!-- text report -->
            <h1 class="text-3xl font-bold mb-2">Text Report</h1>

            <h2 class="sm:text-2xl text-sm font-light">
              Word Count: {{ theResponse[0].wordCount }}
            </h2>
            <h2 class="sm:text-2xl text-sm font-light">
              Character Count: {{ theResponse[0].characterCount }}
            </h2>
            <h2 class="sm:text-2xl text-sm font-light">
              Paragraphs: {{ theResponse[0].paragraphs.length }}
            </h2>
            <h2 class="sm:text-2xl text-sm font-light">
              Estimated Pages: {{ theResponse[0].estimatedPages }}
            </h2>
            <h2 class="sm:text-2xl text-sm font-light">Most Frequent Words:</h2>
            <ul class="list-disc pl-4">
              <li class="text-lg">
                {{
                  JSON.stringify(theResponse[0].mostFrequentWords)
                    .replace(/"/g, "")
                    .replace(/,/g, ", ")
                    .replace(/:/g, ": ")
                    .replace(/{/g, "{ ")
                    .replace(/}/g, " }")
                }}
              </li>
            </ul>
            <h2 class="sm:text-2xl text-sm font-light">Sentiment:</h2>
            <p class="text-lg">Score: {{ theResponse[0].sentiment.score }}</p>
            <p class="text-lg">Comparative: {{ theResponse[0].sentiment.comparative }}</p>
            <p class="text-lg">Sentiment: {{ theResponse[0].sentiment.sentiment }}</p>
          </div>
          <div
            v-if="theResponse[0]?.paragraphs.length > 0"
            class="bg-gray-200 p-4 rounded"
          >
            <h2 class="text-xl font-bold mb-2">Paragraphs:</h2>
            <div
              v-for="(paragraph, index) in theResponse[0].paragraphs"
              :key="index"
              class="mb-4"
            >
              <p class="text-lg">{{ paragraph }}</p>
            </div>
          </div>
          <div v-else class="text-white text-center">
            <p>No paragraphs found.</p>
          </div>
        </div>
      </div>
    </div>
    <!-- made by franc using my package this https://www.npmjs.com/package/text-analysis-api -->
    <span
      class="sm:text-xl flex justify-center items-center mx-auto py-2 text-center text-nuetral-600 font-bold"
    >
      Made using

      <a
        href="https://www.npmjs.com/package/text-analysis-api"
        target="_blank"
        class="text-blue-500 italic mx-2"
      >
        text-analysis-api
      </a>
      <span class="text-neutral-500 mx-2">by</span>
      <a
        href="https://www.npmjs.com/~francis254"
        target="_blank"
        class="text-neutral-600"
      >
        franc
      </a>
    </span>
  </div>
</template>

<script>
import AOS from "aos";
import "aos/dist/aos.css";
import analyzeText from "text-analysis-api";
export default {
  data() {
    return {
      isLoading: false,
      text: "",
      theResponse: [],
    };
  },
  beforeMount() {
    created: {
      AOS.init();
    }
  },

  methods: {
    async analyzeText() {
      this.theResponse = [];
      if (!this.text) {
        alert("Please enter some text");

        return;
      }
      this.isLoading = true;
      const response = await analyzeText(this.text);

      setTimeout(() => {
        this.isLoading = false;

        this.theResponse.push(response);
        console.log("theResponse", this.theResponse[0]);
        this.text = "";
      }, 1000);
    },
  },
};
</script>

<style scoped></style>
