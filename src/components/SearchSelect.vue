<template>
  <on-click-outside :do="close">
    <div class="search-select" :class="{'is-active':isOpen}">
      <button ref="button" type="button" @click="open" class="search-select-input">
        <span v-if="value !== null">{{value}}</span>
        <span v-else class="search-select-placeholder">Select a band...</span>
      </button>
      <div v-if="isOpen" ref="dropdown" class="search-select-dropdown">
        <input
          v-model="search"
          @keydown.escape="close"
          @keydown.down="highlightNext"
          @keydown.up="highlightPrev"
          @keydown.enter.prevent="selectHighlighted"
          @keydown.tab.prevent
          class="search-select-search"
          ref="searchInput"
        >
        <ul v-if="filteredOptions.length > 0" ref="options" class="search-select-options">
          <li
            @click="select(option)"
            class="search-select-option"
            :class="{'is-active': index === highlightedIndex}"
            v-for="(option,index) in filteredOptions"
            :key="option"
          >{{option}}</li>
        </ul>
        <div
          v-if="filteredOptions.length === 0"
          class="search-select-empty"
        >No results found for '{{search}}'</div>
      </div>
    </div>
  </on-click-outside>
</template>
<script>
import OnClickOutside from "./OnClickOutside.vue";
import Popper from "popper.js";
export default {
  props: ["value", "options", "filterFunction"],
  data() {
    return {
      isOpen: false,
      search: "",
      highlightedIndex: 0
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
        this.setupPopper();
      });
    },
    select(option) {
      this.$emit("input", option);
      this.search = "";
      this.close();
    },
    close() {
      if (!this.isOpen) {
        return;
      }
      this.isOpen = false;
      this.highlightedIndex = 0;
      this.$refs.button.focus();
    },
    highlightNext() {
      this.highlightedIndex++;
      if (this.highlightedIndex > this.options.length - 1) {
        this.highlightedIndex = 0;
      }
      this.$refs.options.children[this.highlightedIndex].scrollIntoView({
        block: "nearest"
      });
    },
    highlightPrev() {
      this.highlightedIndex--;
      if (this.highlightedIndex < 0) {
        this.highlightedIndex = this.options.length - 1;
      }
      this.$refs.options.children[this.highlightedIndex].scrollIntoView({
        block: "nearest"
      });
    },
    selectHighlighted() {
      this.select(this.filteredOptions[this.highlightedIndex]);
      this.close();
    },
    setupPopper() {
      if (this.popper === undefined) {
        this.popper = new Popper(this.$refs.button, this.$refs.dropdown, {
          placement: "bottom"
        });
      } else {
        this.popper.scheduleUpdate();
      }
    }
  },
  beforeDestroy() {
    this.popper.destroy();
  },
  components: {
    OnClickOutside
  }
};
</script>

