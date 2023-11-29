<template>
  <h1 class="text-3xl font-bold">Group Brands</h1>
  <p v-if="brands.length === 0" class="mt-8 text-red-600">
    Please add first some brands.
  </p>
  <div class="mt-8" v-else>
    <p class="text-base">
      Using the brand list previously, add the brands in the corresponding
      groups below (separated per line), and make sure the texts match in the
      brand list table.
    </p>
    <form class="mt-8" @submit.prevent="finalizeGroups">
      <div class="flex justify-between border-b-2 pb-2 items-center">
        <div>
          <label for="country" class="text-sm"
            >Enter country code for label prefix (without letters):
          </label>
          <input
            type="number"
            id="country"
            v-model="countryCode"
            class="ml-3 w-12 border-2 rounded border-solid border-black"
          />
        </div>
        <button
          class="bg-blue-400 w-28 p-2 text-sm rounded font-bold hover:bg-blue-200"
        >
          Submit
        </button>
      </div>

      <p v-if="showError" class="text-red-500 mt-2 font-bold">
        Please make sure country code and at least one group below is filled.
      </p>
      <div class="flex flex-row pt-2 flex-wrap">
        <group-input
          v-for="(value, key) in groups"
          :group="{ code: key, title: value }"
          @add-brands-to-group="addBrandsToGroup"
        ></group-input>
      </div>
    </form>
  </div>
</template>

<script>
import GroupInput from "./GroupInput.vue";
export default {
  inheritAttrs: false,
  props: ["brands"],
  components: {
    "group-input": GroupInput,
  },
  provide() {
    return {
      brands: this.brands,
    };
  },
  emits: ["add-brands-to-group", "updateList"],
  data() {
    return {
      showError: false,
      countryCode: "",
      groups: {
        "01": "Current account",
        "02": "Savings account",
        "03": "Long-term deposit",
        "04": "Investment",
        "05": "Pension/Retirement",
        "06": "Credit card",
        "07": "Consumer loan",
        "08": "Mortgage",
        "09": "Insurance",
        10: "Overdraft",
      },
      entries: {},
    };
  },
  methods: {
    addBrandsToGroup(code, labels) {
      if (labels === "remove") {
        if (Object.keys(this.entries).length > 0) delete this.entries[code];
      } else {
        this.entries[code] = labels;
      }
    },
    finalizeGroups() {
      if (this.countryCode === "" || Object.keys(this.entries).length == 0) {
        this.errorShown = true;
      } else {
        const finalEntries = {};

        for (const [key, value] of Object.entries(this.entries)) {
          finalEntries[`r${this.countryCode}${key}`] = value;
        }

        this.$emit("updateList", "entries", finalEntries);
      }
    },
  },
};
</script>
