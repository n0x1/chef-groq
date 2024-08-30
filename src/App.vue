<template>
<div>
  <Topbar />
  <div class="flex-col">
    <Input label='Ingredients:' id='ingredients' use='ingredients'/>
    <button @click="main" id="promptbut">Prompt</button>
  </div>
  <p id="response" v-html="richTextContent"></p>

</div>
</template>

<script setup lang="ts">
import Input from "./components/InputField.vue"
import Topbar from  "./components/TopBar.vue"
import { ref } from "vue"

import Groq from "groq-sdk";
const key = import.meta.env.VITE_GROQ_API_KEY;
const groq = new Groq({ apiKey: key, dangerouslyAllowBrowser: true});

async function main() {
  const ingredientsOwned = document.getElementById("ingredients").textContent
  const chatCompletion = await getGroqChatCompletion(ingredientsOwned);
  const response = (chatCompletion.choices[0]?.message?.content || "");
  if ( document.getElementById("response").textContent != "")
    document.getElementById("response").textContent = ""

  document.getElementById("response").textContent = response
}

async function getGroqChatCompletion(ingredientsOwned) {
  return groq.chat.completions.create({
    messages: [
      {
        role: "user",
        content: `You are a professional chef named groq. Given a list of ingredients, provide a healthy, sustainable, and easy recipe, along with the steps to make it with the given ingredients. Refrain from suggesting any unowned ingredients, unless they are necessary and/or highly nutritious. Do not use bold or formatting with **. You may only use line breaks to format. Here are the owned ingredients: "${ingredientsOwned}". If the user did not provide valid ingredients, concisely suggest them to provide them."`
      },
    ],
    model: "llama3-8b-8192",
  });


}
</script>

<script lang='ts'>
export default {
  data() {
    return {
      richTextContent: "<p><i><b>...</b></i></p>"
    };
  }
};
</script>



