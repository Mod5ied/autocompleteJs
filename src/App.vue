<script>
import { reactive, toRefs } from "@vue/reactivity";
import Autocomplete from "./components/Autocomplete.vue";
export default {
  setup() {
    let data = reactive({
      items: [
        "Arab",
        "America",
        "Argentina",
        "Bulgaria",
        "Belgium",
        "China",
        "Dubai",
        "Japan",
        "Germany",
        "Ghana",
        "Italy",
        "India",
        "Nigeria",
        "Russia",
        "Spain",
        "Singapore",
        "South Africa",
        "Sweden",
        "Uganda",
      ],
    });

    return {
      ...toRefs(data),
    };
  },
  components: { Autocomplete },
};
</script>

<template>
  <Autocomplete :items="items" />
</template>

<style></style>
