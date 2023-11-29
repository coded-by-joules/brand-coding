<template>
  <the-header :tab="activeTab" @switch-tab="switchTab"></the-header>
  <main class="container mx-auto p-5 max-w-screen-lg">
    <KeepAlive>
      <component
        :is="activeTab"
        v-bind="currentProperties"
        v-on="currentEvents"
      ></component>
    </KeepAlive>
  </main>
</template>

<script>
import TheHeader from "./components/TheHeader.vue";

import BrandCodes from "./components/brand_codes/BrandCodes.vue";
import GroupBrands from "./components/group_brands/GroupBrands.vue";
import FinalOutput from "./components/final_output/FinalOutput.vue";

export default {
  components: {
    "the-header": TheHeader,
    "brand-codes": BrandCodes,
    "group-brands": GroupBrands,
    "final-output": FinalOutput,
  },
  emits: ["switch-tab", "updateList"],
  data() {
    return {
      activeTab: "brand-codes",
      brandList: [],
      entriesList: {},
    };
  },
  computed: {
    currentProperties() {
      if (this.activeTab == "brand-codes" || this.activeTab == "group-brands") {
        return { brands: this.brandList };
      } else {
        return { entries: this.entriesList };
      }
    },
    currentEvents() {
      if (this.activeTab == "brand-codes" || this.activeTab == "group-brands") {
        return { updateList: this.updateListHandler };
      } else {
        return {};
      }
    },
  },
  methods: {
    switchTab(newTab) {
      this.activeTab = newTab;
    },
    updateListHandler(item, lists) {
      if (item === "brands") {
        this.brandList = lists;
      } else if (item === "entries") {
        this.entriesList = lists;
        this.activeTab = "final-output";
        console.log(this.entriesList);
      } else {
        this.brandList = [];
        this.entriesList = {};
        this.activeTab = "brand-codes";
      }
    },
  },
};
</script>
