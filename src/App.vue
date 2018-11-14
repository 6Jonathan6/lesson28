<template>
  <div id="app" class="font-sans text-black min-h-screen bg-grey-darker p-8">
    <div class="max-w-sm mx-auto bg-white rounded shadow-lg p-8">
      <div class="mb-4">
        <label class="form-label mb-2">Favorite Thrash Band</label>
        <search-select
          v-model="selectedBand"
          :options="options"
          :filter-function="applySearchFilter"
        />
      </div>
    </div>
  </div>
</template>
<script>
import SearchSelect from "./components/SearchSelect.vue";
export default {
  name: "app",
  data() {
    return {
      selectedBand: null,
      options: [
        "Anthrax",
        "Dark Angel",
        "Death Angel",
        "Destruction",
        "Sepultura",
        "Exodus",
        "Flotsam and Jetsam",
        "Kreator",
        "Megadeth",
        "Metallica",
        "Overkill",
        "Slayer",
        "Testament"
      ]
    };
  },
  methods: {
    applySearchFilter(search, options) {
      if (search === "" || search === " ") {
        return options;
      }
      return options.filter(option =>
        option.toLowerCase().startsWith(search.toLowerCase())
      );
    }
  },
  components: { SearchSelect }
};
</script>

<style src="./assets/css/app.css"/>
