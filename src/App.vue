<template>
  <form action="" @submit.prevent="getLink">
    <input
      type="text"
      name="link"
      id="link"
      placeholder="Your URL"
      v-model="link"
    />
    <button>Shorten URL</button>
    <input type="text" placeholder="Your shortened URL" :value="shortLink">
  </form>
</template>




<script>
import { ref } from "vue";
// import {useStore} from 'vuex'

export default {
  setup() {
    const link = ref("");
    const shortLink = ref("")

    async function getLink() {
      const response = await fetch("https://api-ssl.bitly.com/v4/shorten", {
        method: "POST",
        headers: {
          "Authorization": 'Bearer 6e9551a514b1f88c27de0f92bcb9bd956baaf8e7',
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          long_url: link.value,
          domain: "bit.ly",
        }),
      });
      const data = await response.json();
      shortLink.value = data.link;

    }

    //Access token: 6e9551a514b1f88c27de0f92bcb9bd956baaf8e7

    return { link, shortLink, getLink };
  },
};
</script>




<style scoped>
* {
  font-family: Roboto, sans-serif;
  font-size: 2rem;
}

form {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 50%;
}

form button {
  color: white;
  background-color: orange;
  border: none;
  border-radius: 5px;
  padding: 0.5rem;
  font-weight: bold;
  display: block;
  margin: 30px auto;
}

input {
  height: 3rem;
  display: block;
  border: 2px solid orange;
  border-radius: 5px;
  width: 100%;
}
</style>