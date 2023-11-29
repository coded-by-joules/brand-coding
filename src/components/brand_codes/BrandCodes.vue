<template>
  <h1 class="text-3xl font-bold">Brand List Setup</h1>
  <form class="mt-8" @submit.prevent="generateCodes" v-if="!showResult">
    <p class="text-base">
      <strong>Step 1:</strong> Insert the brand list in XML format, and click
      "Generate List".
    </p>
    <p class="text-red-600 font-bold mt-3" v-if="errorText !== ''">
      {{ errorText }}
    </p>
    <textarea
      rows="10"
      class="border-2 rounded border-black mt-3 w-full"
      placeholder="Add the brands in <row label='rXX'></row> format"
      v-model="rawText"
    ></textarea>
    <button
      class="mt-3 p-3 rounded text-white bg-violet-600 hover:bg-violet-500"
    >
      Generate List
    </button>
  </form>

  <div class="mt-8" v-else>
    <p class="text-base">
      <strong>Step 2:</strong> Double check the result below, and if you wish to
      modify the list, click the "Modify List" button, or click the
      <strong>"Group Brands"</strong> in the navigation bar.
    </p>
    <div class="flex justify-center items-center">
      <button
        @click="modifyList"
        class="mt-3 text-center p-3 rounded text-white bg-violet-600 hover:bg-violet-500"
      >
        Modify List
      </button>
    </div>
    <table class="table-fixed w-3/4 mt-4 mx-auto">
      <thead>
        <tr>
          <th class="p-1 text-center bg-violet-500 text-white">Label</th>
          <th class="p-1 text-center bg-violet-500 text-white">Text</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="brand in brandList" :class="rowBG(brand)">
          <td class="p-1 text-center">{{ brand.label }}</td>
          <td class="p-1">{{ brand.text }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: ["brands"],
  emits: ["updateList"],
  data() {
    return {
      rawText: "",
      errorText: "",
      rawList: [],
      brandList: [],
      showResult: false,
    };
  },
  methods: {
    generateCodes() {
      const getLines = this.rawText.trim().split("\n");
      this.rawList = getLines.map((item) => item.trim());
      this.brandList = [];
      this.errorText = "";

      this.rawList.forEach((item) => {
        try {
          const getlabel = item.match(/label=\"([a-z]*[0-9]*)\"/);
          const gettext = item.match(/<(.*?) [^>]+>(.*?)<\/(.*?)>/);

          this.brandList.push({
            label: getlabel[1],
            text: gettext[2],
          });
        } catch (error) {
          this.errorText =
            "Please double check your XMLs since we can't parse it";
          console.log(error);
        }
      });

      if (this.brandList.length > 0) {
        this.showResult = true;
        this.$emit("updateList", "brands", this.brandList);
      }
    },
    modifyList() {
      this.showResult = false;
    },
    rowBG(brand) {
      if (this.brandList.indexOf(brand) % 2 === 0) {
        return "bg-violet-100";
      } else {
        return "bg-violet-300";
      }
    },
  },
};
</script>
