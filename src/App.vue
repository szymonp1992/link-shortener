<template>
  <div>
    <form action="" @submit.prevent="getLink">
      <input
        type="text"
        name="link"
        id="link"
        placeholder="Your URL"
        v-model="link"
      />
      <button>Shorten URL</button>
    </form>
    <input
      type="text"
      id="shortURL"
      placeholder="Your shortened URL"
      :value="shortLink"
    />
    <button class="btn" @click="copyShortUrl" data-clipboard-target="#shortURL">
      Copy
    </button>
    <p v-if="isActive">Copied</p>
  </div>
</template>



<script>
import { ref } from "vue";
import ClipboardJS from "../clipboardjs/src/clipboard";

export default {
  setup() {
    const link = ref("");
    const shortLink = ref("");
    const isActive = ref(false);

    async function getLink() {
      isActive.value = false;
      const response = await fetch("https://api-ssl.bitly.com/v4/shorten", {
        method: "POST",
        headers: {
          Authorization: "Bearer 6e9551a514b1f88c27de0f92bcb9bd956baaf8e7",
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

    const clipboard = new ClipboardJS(".btn");

    clipboard.on("success", () => {
      isActive.value = true;
      setTimeout(() => {
        isActive.value = false;
      }, 2000);
    });

    return { link, shortLink, isActive, getLink };
  },
};
</script>




<style scoped>
* {
  font-family: Roboto, sans-serif;
  font-size: 2rem;
}

div {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 50%;
}

button {
  color: white;
  background-color: orange;
  border: none;
  border-radius: 5px;
  padding: 0.5rem 2rem;
  font-weight: bold;
  display: block;
  margin: 1rem auto;
}

button:hover {
  cursor: pointer;
}

input {
  display: block;
  border: 2px solid orange;
  border-radius: 5px;
  width: 100%;
  padding: 1rem;
}

p {
  position: relative;
  font-size: 1rem;
  text-align: center;
  margin: 0;
}
</style>