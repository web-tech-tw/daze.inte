<template>
  <div class="relative">
    <button aria-expanded="false"
      class="text-gray-500 group bg-white rounded-md inline-flex items-center text-base font-medium hover:text-gray-900 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
      type="button" @click="handleDropdownClick">
      <span>{{ props.name }}</span>
      <chevron-down-icon class="text-gray-400 ml-2 h-5 w-5 group-hover:text-gray-500" />
    </button>
    <div
      class="absolute z-10 -ml-4 mt-3 transform px-2 w-screen max-w-md sm:px-0 lg:ml-0 lg:left-1/2 lg:-translate-x-1/2">
      <div v-show="isDropdownOpened" class="rounded-lg shadow-lg ring-1 ring-black ring-opacity-5 overflow-hidden">
        <div class="relative grid gap-6 bg-white px-5 py-6 sm:gap-8 sm:p-8">
          <app-header-normal-menu-dropdown-item :name="child.name" :description="child.description" :icon="child.icon"
            @click="handleItemClick(child)" v-for="(child, index) in props.children" :key="index" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, inject, watch } from "vue";

import { ChevronDownIcon } from "@heroicons/vue/24/solid"

import AppHeaderNormalMenuDropdownItem from './AppHeaderNormalMenuDropdownItem.vue';

const isDropdownOpened = ref(false);

const parentMenuState = inject('parent-menu-state')
watch(parentMenuState, (value) => {
  if (!value) {
    isDropdownOpened.value = false;
  }
});

const handleDropdownClick = () => {
  isDropdownOpened.value = !isDropdownOpened.value;
  if (isDropdownOpened.value) {
    parentMenuState.value = true;
  }
};

const handleItemClick = (item) => {
  parentMenuState.value = false;
  item.onClick();
}

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  children: {
    type: Array,
    require: true,
  }
});
</script>
