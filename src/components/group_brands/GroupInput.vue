<template>
  <div :class="['p-5', 'border-2', 'rounded', 'w-1/4', groupStatus]">
    <h4 class="text-1xl font-bold">{{ group.title }}</h4>
    <p class="mt-2 text-sm font-bold text-red-500" v-if="errorText !== ''">
      {{ errorText }}
    </p>
    <textarea
      rows="10"
      class="mt-2 border-2 rounded text-sm w-full"
      v-model="rawData"
      v-if="!showResult"
    ></textarea>
    <div v-else class="max-h-52 overflow-y-auto">
      <table class="table-auto mt-2 w-full text-sm">
        <thead>
          <tr>
            <th class="bg-violet-500">Label</th>
            <th class="bg-violet-500">Text</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="result in results" :class="rowBG(result)">
            <td class="p-1 text-center">{{ result.label }}</td>
            <td class="p-1">{{ result.title }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <button
      type="button"
      v-if="!showResult && !confirmed"
      class="text-sm mt-2 bg-violet-400 p-2 rounded hover:bg-violet-300"
      @click="verifyList"
    >
      Verify
    </button>
    <button
      type="button"
      v-if="showResult && !confirmed"
      class="text-sm mt-2 mr-2 bg-violet-400 p-2 rounded hover:bg-violet-300"
      @click="modifyList"
    >
      Modify
    </button>
    <button
      type="button"
      v-if="showResult && !confirmed"
      class="text-sm mt-2 bg-green-400 p-2 rounded hover:bg-green-300"
      @click="confirmList"
    >
      Confirm
    </button>
    <button
      type="button"
      v-if="showResult && confirmed"
      class="text-sm mt-2 bg-violet-400 p-2 rounded hover:bg-violet-300"
      @click="editList"
    >
      Re-edit
    </button>
  </div>
</template>

<script>
export default {
  props: ["group"],
  inject: ["brands"],
  data() {
    return {
      rawData: "",
      results: [],
      showResult: false,
      confirmed: false,
      errorText: "",
    };
  },
  computed: {
    groupStatus() {
      if (this.confirmed) {
        return { "border-green-400": true };
      }
      return { "border-gray-300": true };
    },
  },
  methods: {
    verifyList() {
      const getLines = this.rawData.split("\n").filter((element) => element);
      if (getLines.length > 0) {
        const rawList = getLines.map((item) => item.trim());
        let foundCount = 0;

        rawList.forEach((item) => {
          let matchFound = false;

          this.brands.forEach((brand) => {
            if (
              item.toString().toLowerCase() ===
              brand.text.toString().toLowerCase()
            ) {
              matchFound = true;
              this.results.push({
                label: brand.label,
                title: item,
                found: true,
              });
              foundCount++;
            }
          });

          if (!matchFound) {
            this.results.push({
              label: "",
              title: item,
              found: false,
            });
          }
        });

        this.showResult = true;
        if (foundCount != rawList.length) {
          this.errorText = "Entries marked in red will not be included";
        }
      } else {
        this.errorText = "Please enter brand lists below";
      }
    },
    confirmList() {
      const resultLabels = this.results
        .filter((item) => item.found)
        .map((item) => item.label);

      if (resultLabels.length > 0) {
        this.confirmed = true;
        this.$emit(
          "add-brands-to-group",
          this.group.code,
          resultLabels.join(",")
        );
      } else {
        this.errorText =
          "Please modify the list to have at least one found label";
      }
    },
    editList() {
      this.confirmed = false;
      this.showResult = false;
      this.results = [];
      this.errorText = "";
      this.$emit("add-brands-to-group", this.group.code, "remove");
    },
    modifyList() {
      this.showResult = false;
      this.errorText = "";
      this.results = [];
    },
    rowBG(brand) {
      if (!brand.found) return ["bg-red-300"];
    },
  },
};
</script>
