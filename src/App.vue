<template>
  <TabsWrapper :tabTitles="tabTitles">
    <template #deleteIcon="{ tabIndex }">
      <IconDel @click="deleteItemDay(itineraryDays[tabIndex])" />
    </template>
    <Tab
      v-for="(day, dayIndex) in itineraryDays"
      :key="dayIndex"
      :title="day.name"
    >
      {{ day.title }}
      <TabsWrapper :tabTitles="itineraryTabs(day)">
        <Tab
          v-for="(itinerary, itineraryIndex) in day.items"
          :key="itineraryIndex"
          :title="itinerary.name"
        >
          <div v-for="(item, itemIndex) in itinerary.data" :key="itemIndex">
            <input type="checkbox" v-model="item.check" />
            {{ item.description }}
          </div>
        </Tab>
      </TabsWrapper>
    </Tab>
  </TabsWrapper>
  <div class="mt-15">
    <a title="Agregar" @click="addItemDay()">
      <IconAdd />
      <span class="text-icon">Add New Day</span>
    </a>
  </div>
</template>

<script>
import TabsWrapper from "./components/TabsWrapper.vue";
import Tab from "./components/Tab.vue";
import { ref, computed } from "vue";
import IconDel from "./components/icons/IconDel.vue";
import IconAdd from "./components/icons/IconAdd.vue";

export default {
  components: {
    TabsWrapper,
    Tab,
    IconDel,
    IconAdd,
  },
  setup() {
    const selectedTitle = ref("");
    const itineraryDays = ref([
      {
        name: "Day 1",
        title: "Water Park",
        items: [
          {
            data: [
              { description: "Round-trip transportation" },
              { check: true, description: "Breakfast" },
              { check: true, description: "Lunch" },
              {
                description: "Admission to Water Park with multiple passport",
              },
              { description: "Medical assistance" },
              { description: "Travel coordinator" },
            ],
            name: "Included",
          },
          {
            data: [{ check: true, description: "Unspecified expenses" }],
            name: "Not Included",
          },
        ],
      },
      {
        name: "Day 2",
        title: "Magic Land",
        items: [
          {
            data: [
              { description: "Round-trip transportation" },
              { description: "Travel coordinator" },
            ],
            name: "Included",
          },
          {
            data: [{ check: true, description: "Unspecified expenses" }],
            name: "Not Included",
          },
        ],
      },
    ]);
    // Tabs Days
    const tabTitles = computed(() =>
      itineraryDays.value.map((day) => day.name)
    );

    const deleteItemDay = (day) => {
      const index = itineraryDays.value.indexOf(day);
      if (index !== 0) {
        itineraryDays.value.splice(index, 1);
        updateDayNames();
        console.log("Go to Tab " + `Day ${index}` + " After Delete");
      }
    };

    const addItemDay = () => {
      // Create a new day object with default values
      const newDay = {
        name: `Day ${itineraryDays.value.length + 1}`,
        title: "",
        description: "",
        items: [
          { name: "Included", data: [] },
          { name: "Not Included", data: [] },
        ],
      };

      // Add the new day to the itineraryDays array
      itineraryDays.value.push(newDay);
      updateDayNames();
    };

    const updateDayNames = () => {
      itineraryDays.value.forEach((day, index) => {
        day.name = `Day ${index + 1}`;
      });
    };

    const itineraryTabs = (day) => day.items.map((itinerary) => itinerary.name);

    return {
      itineraryDays,
      tabTitles,
      itineraryTabs,
      deleteItemDay,
      addItemDay,
    };
  },
};
</script>
