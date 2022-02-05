<template>
  <div class="autocomplete">
    <input
      type="text"
      v-model="search"
      @keydown.up="onKeyUp"
      @keydown.down="onKeyDown"
      @keydown.enter="onEnter"
      @input="onChange"
      placeholder="Search for country"
    />
    <ul v-show="isOpen" class="autocomplete-results">
      <li
        v-for="(result, i) in results"
        :key="i"
        @click="setResult(result)"
        class="result"
        :class="{ 'is-active': i === tracker }"
      >
        {{ result }}
      </li>
    </ul>
  </div>
</template>

<script>
import { reactive, toRefs } from "@vue/reactivity";
import { onMounted, onUnmounted } from "@vue/runtime-core";
export default {
  props: {
    items: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  setup(props) {
    onUnmounted(() => {
      "click", methods.handleOutsideClick;
    });
    onMounted(() => {
      "click", methods.handleOutsideClick;
    });
    let data = reactive({
      search: "",
      results: [],
      isOpen: true,
      tracker: -1,
    });

    let methods = reactive({
      filterResults: () => {
        return (data.results = props.items.filter(
          (item) => item.toLowerCase().indexOf(data.search.toLowerCase()) > -1
        ));
      },
      handleOutsideClick: (e) => {
        if (!$el.contains(e.target)) {
          return (data.tracker = -1), (isOpen = false);
        }
      },
      onChange: () => {
        methods.filterResults();
        data.isOpen = true;
      },
      onEnter: () => {
        return (
          (data.search = data.results[data.tracker]),
          (data.isOpen = false),
          (data.tracker = -1)
        );
      },
      onKeyDown: () => {
        if (data.tracker < data.results.length) {
          data.tracker += 1;
        }
      },
      onKeyUp: () => {
        if (data.tracker < data.results.length) {
          data.tracker -= 1;
        }
      },
      setResult: (e) => {
        data.search = e;
        data.isOpen = false;
      },
    });

    return {
      ...toRefs(data),
      ...toRefs(methods),
    };
  },
};
</script>

<style scoped>
.autocomplete {
  margin: 3rem 5rem;
}
.autocomplete-results {
  width: 30%;
  list-style-type: none;
}
.result {
  padding: 0.3rem;
}
.result.is-active,
.result:hover {
  background-color: #4aae9b;
  color: white;
}
input {
  width: 70%;
  padding: 0.8rem 3.5rem;
  border: 1px solid hsl(0, 0%, 93%);
  border-radius: 0.4rem;
  outline: none;
  box-shadow: 0 1px 3px 0 rgba(197, 196, 196, 0.1);
  transition: 0.4s ease-out;
  font: 600 1rem;
  color: #414549f6;
}
input:hover,
input:focus {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
  transform: scale(1.02);
  transition: 0.3s ease-in-out;
}
</style>
