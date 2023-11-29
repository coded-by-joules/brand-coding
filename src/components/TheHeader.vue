<template>
  <header class="bg-violet-600 py-5 text-center">
    <h1 class="text-3xl font-bold text-white">REBEX Brand Coding</h1>
    <nav class="mt-5">
      <ul class="list-none text-white font-bold">
        <li :class="['listItem', { active: this.tab === 'brand-codes' }]">
          <button @click="toggleTab('brand-codes')">Brand List</button>
        </li>
        <li :class="['listItem', { active: this.tab === 'group-brands' }]">
          <button @click="toggleTab('group-brands')">Group Brands</button>
        </li>
        <li :class="['listItem', { active: this.tab === 'final-output' }]">
          <button @click="toggleTab('final-output')">Final Output</button>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  props: ["tab"],
  methods: {
    toggleTab(tab) {
      this.$emit("switch-tab", tab);
    },
  },
};
</script>

<style scoped>
.listItem {
  @apply inline-block mx-3 px-3 py-1 text-center rounded;
}
.listItem:hover {
  @apply bg-violet-500;
}
.active {
  @apply border-2 border-white;
}
</style>
