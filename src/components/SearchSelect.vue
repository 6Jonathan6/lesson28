<template>
    <div class="search-select" :class="{'is-active':isOpen}">
        <button ref="button" type="button" @click="open" class="search-select-input">
            <span v-if="value !== null">{{value}}</span>
            <span v-else class="search-select-placeholder">Select a band...</span>
        </button>
        <div v-if="isOpen" class="search-select-dropdown">
            <input v-model="search" class="search-select-search" ref="searchInput">
            <ul v-if="filteredOptions.length > 0" class="search-select-options">
                <li
                    @click="select(option)"
                    class="search-select-option"
                    v-for="option in filteredOptions"
                    :key="option"
                >{{option}}</li>
            </ul>
            <div
                v-if="filteredOptions.length === 0"
                class="search-select-empty"
            >No results found for '{{search}}'</div>
        </div>
    </div>
</template>
<script>
export default {
  props: ["value", "options", "filterFunction"],
  data() {
    return {
      isOpen: false,
      search: ""
    };
  },
  computed: {
    filteredOptions() {
      return this.filterFunction(this.search, this.options);
    }
  },
  methods: {
    open() {
      this.isOpen = this.isOpen = true;
      this.$nextTick(() => {
        this.$refs.searchInput.focus();
      });
    },
    select(option) {
      this.$emit("input", option);
      this.search = "";
      this.close();
    },
    close() {
      this.isOpen = false;
      this.$refs.button.focus();
    }
  }
};
</script>
