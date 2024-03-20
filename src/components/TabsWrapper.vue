<template>
{{selectedTitle}}
  <div class="tabs">
    <ul class="tabs__header">
      <li
        v-for="(title, tabIndex) in tabTitles"
        :key="title"
        :class="{ selected: title === selectedTitle }"
        @click="selectTab(title)"
      >
        {{ title }}
        <!-- Slot for delete icon within each tab title -->
        <slot name="deleteIcon" :tabIndex="tabIndex"></slot>
      </li>
    </ul>
    <slot />
  </div>
</template>

<script>
import { ref, provide } from "vue";

export default {
  props: {
    tabTitles: Array,
  },
  setup(props) {
    const selectedTitle = ref(props.tabTitles[0]);

    const selectTab = (title) => {
      selectedTitle.value = title;
    };

    provide("selectedTitle", selectedTitle);

    return {
      selectedTitle,
      selectTab,
    };
  },
};
</script>

<style scoped>
.tabs {
  margin: 0 auto;
}
.tabs__header {
  list-style: none;
  padding: 0;
  display: flex;
}
.tabs__header li {
  padding: 10px 20px;
  margin-right: 10px;
  background-color: #ddd;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.4s all ease-out;
}

.tabs__header li.selected {
  background-color: red;
  color: white;
}
</style>
