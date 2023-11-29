<template>
  <h1 class="text-3xl font-bold">Final Output</h1>
  <div v-if="Object.keys(entries).length > 0">
    <div class="mt-10 border-b-2 border-gray-300 pb-3 flex justify-between">
      <p>Here is the generated XMLs to be added to the xBrandCodes dummy.</p>
      <button
        type="button"
        class="bg-blue-400 w-28 p-2 text-sm rounded font-bold hover:bg-blue-200"
        @click="startOver"
      >
        Start Over
      </button>
    </div>
    <textarea
      rows="15"
      class="border-2 border-black rounded mt-4 w-full"
      readonly
      >{{ showEntries }}</textarea
    >
  </div>
  <p class="mt-10 font-bold text-red-500" v-else>
    Please generate results in "Group Brands"
  </p>
</template>

<script>
export default {
  inheritAttrs: false,
  props: ["entries"],
  data() {
    return {
      xmlData: "",
    };
  },
  computed: {
    showEntries() {
      let entriesStr = "";
      console.log("watching");
      console.log(this.entries);
      for (const [key, value] of Object.entries(this.entries)) {
        entriesStr += `<row label="${key}">${value}</row>\n`;
      }

      return entriesStr;
    },
  },
  methods: {
    startOver() {
      location.reload();
    },
  },
};
</script>
