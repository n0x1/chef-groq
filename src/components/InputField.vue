<template>
  <div style="display:flex;gap:2px;align-items:center;flex-direction:column">
    <label v-if="label">{{ label }}</label>
    <div 
      contenteditable="true"
      ref="editableDiv"
      @input="updateContent"
      class="editable"
      style="padding:10px; margin-bottom:5px; height:fit-content; white-space: pre-wrap; max-height:200px; width:fit-content; max-width:250px; min-width:100px; background-color:#515254;border: 2px solid black;border-radius:4px;overflow:scroll;"
    ></div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  props: {
    label: {
      type: String,
      default: "",
    },
    use: {
        type: String,
        default: "ingredients",
    },
  },
computed: {
    isCost() {
      return this.use === 'ingredients';
    }
  },

  setup() {
    const editableDiv = ref(null);

    const updateContent = () => {
      if (editableDiv.value) {
    let content = editableDiv.value.innerText
    content = content.replace(/\n/g, ' : '); // line

        console.log("content:", content);
      } else {
        console.error("editableDiv is null");
      }
    };

    onMounted(() => {
      if (editableDiv.value) {
        console.log("Mounted");
      }
    });

    return {
      editableDiv,
      updateContent,
    };
  },
};
</script>
