<template>
  <div class="input-holder">
    <input v-model="titleInput" placeholder="Enter title" />
    <input v-model="urlInput" placeholder="Enter url" />
    <input v-model="descriptionInput" placeholder="Enter description" />
    <input v-model="imageInput" placeholder="Enter image url" />

    <input v-model="parseUrlInput" placeholder="Enter website url" />
    <button @click="parseMetaData" :class="{ 'loading': loading }" class="parse-button">
      {{ loading ? 'Loading...' : 'Parse' }}
    </button>
    <button @click="parseMetaDataTest">Passs</button>
    {{ myData }}
  </div>

  <div class="div-holder">
    <div class="facebook-div">
      <FacebookPreview :metaProps="metaProps" />
    </div>
    <div class="twitter-div">
      <TwitterPreview :metaProps="metaProps" />
    </div>
  </div>
</template>

<script setup>
import axios from "axios"
import FacebookPreview from "./facebook-preview.vue";
import TwitterPreview from "./twitter-preview.vue";
import { ref, watchEffect } from "vue";
import fetchHeadTags from 'head-meta';

const myData = ref(null)
// fetchHeadTags('https://pratikchakraborty.in')
//     .then(tags => console.log(tags, "sdk"))
//     .catch(error => console.error(error));



const loading = ref(false);

const titleInput = ref("");
const urlInput = ref("");
const descriptionInput = ref("");
const imageInput = ref("");
const parseUrlInput = ref("");

const parseMetaData = async () => {
  try {
    loading.value = true;
    let reqOptions = {
      url: `https://fetch-web-tags.onrender.com/v1/fetchtags?url=${parseUrlInput.value}`,
      method: "GET",
      headers: {},
    };

    let response = await axios.request(reqOptions);

    // Access the necessary properties from the API response
    const metaTags = response.data.metaTags;

    // Update the input fields with the obtained values
    titleInput.value = metaTags && metaTags["og:title"] ? metaTags["og:title"] : "";
    const url = new URL(metaTags && metaTags["og:url"] ? metaTags["og:url"] : "");
    urlInput.value = url.hostname;
    descriptionInput.value = metaTags && metaTags["og:description"] ? metaTags["og:description"] : "";
    imageInput.value = metaTags && metaTags["og:image"] ? metaTags["og:image"] : "";
  } catch (error) {
    console.error("Error parsing meta data:", error);
  } finally {
    loading.value = false;
  }
};

const metaProps = ref({
  title: titleInput.value,
  url: urlInput.value,
  description: descriptionInput.value,
  image: imageInput.value,
});

// Update metaProps when input values change
watchEffect(() => {
  metaProps.value = {
    title: titleInput.value,
    url: urlInput.value,
    description: descriptionInput.value,
    image: imageInput.value,
  };
});

const parseMetaDataTest = async () => {
  try {
    const tags = await fetchHeadTags('https://www.pratikchakraborty.in/');
    console.log('tags', tags);
    myData.value = tags;
  } catch (error) {
    console.error('Error fetching meta data:', error);
  }
};
</script>

<style scoped>
input {
  padding-top: 10px;
  padding-bottom: 10px;
}

.parse-button {
  border-radius: 6px;
  padding: 10px 16px 10px 16px;
  color: aliceblue;
  background-color: #07a0c3;
  border: 0;
  font-family: Poppins;
  font-weight: bolder;
  font-size: 18px;
}

.parse-button:hover {
  border-radius: 20px;
  transition: 0.5s;
  padding: 10px 16px 10px 16px;
  color: aliceblue;
  background-color: #086788;
  border: 0;
  font-family: Poppins;
  font-weight: bolder;
  cursor: pointer;
  font-size: 18px;
}

.facebook-div {
  height: 55vh;
  width: 40vw;
  /* border: 1px solid; */
  padding: 0;
  margin: 0;
}

.twitter-div {
  height: 55vh;
  width: 40vw;
  border: 1px solid rgb(126, 126, 126);
  border-radius: 24px;
  padding: 0;
  margin: 0;
}

.input-holder {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 40vw;
}

.div-holder {
  display: flex;
  gap: 20px;
  margin-top: 30px;
}
</style>
