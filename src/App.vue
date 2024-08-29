<template>
  <div>
    <Input label='Ingredients:' use='ingredients'/>
    <button @click="main">Prompt</button> <!-- No parentheses needed -->
  </div>
</template>

<script setup lang="ts">
import Input from "./components/InputField.vue"
import { ref } from "vue"

import Groq from "groq-sdk";
const key = import.meta.env.VITE_GROQ_API_KEY;
const groq = new Groq({ apiKey: key, dangerouslyAllowBrowser: true});

async function main() {
  const chatCompletion = await getGroqChatCompletion();
  const ingredientsOwned = document.getFromID("ingredients").textcontent
  const availableCash = document.getfromID('cash').text
  console.log(chatCompletion.choices[0]?.message?.content || "");
}

async function getGroqChatCompletion() {
  return groq.chat.completions.create({
    messages: [
      {
        role: "user",
        content: `Provide a healthy recipe and the steps to make it with the given ingredients. You may use at most $${availableCash} to buy any additional (necessary and/or highly nutritious) ingredients, but try to use mostly the ingredients already owned. Here they are, seperated by a colon and spaces: ${ingredientsOwned}`
      },
    ],
    model: "llama3-8b-8192",
  });


}
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
