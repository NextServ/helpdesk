<template>
  <Dialog v-model="show" :options="{ size: '4xl' }">
    <template #body>
      <div class="flex" :style="{ height: 'calc(100vh - 8rem)' }">
        <div class="flex w-52 shrink-0 flex-col bg-gray-50 p-2">
          <h1 class="px-2 pt-2 text-lg font-semibold">Settings</h1>
          <div class="mt-3">
            <button
              v-for="tab in tabs"
              :key="tab.label"
              class="flex h-7 w-full items-center gap-2 rounded px-2 py-1"
              :class="[
                activeTab?.label == tab.label
                  ? 'bg-white shadow-sm'
                  : 'hover:bg-gray-100',
              ]"
              @click="activeTab = tab"
            >
              <component :is="tab.icon" class="h-4 w-4 text-gray-700" />
              <span class="text-base text-gray-800">
                {{ tab.label }}
              </span>
            </button>
          </div>
        </div>
        <div class="flex flex-1 flex-col overflow-scroll p-4">
          <component :is="activeTab.component" v-if="activeTab" />
        </div>
      </div>
    </template>
  </Dialog>
</template>
<script setup lang="ts">
import { Dialog } from "frappe-ui";
import { markRaw, ModelRef, ref } from "vue";
import ImageUp from "~icons/lucide/image-up";
import LucideMail from "~icons/lucide/mail";
import Branding from "./Branding.vue";
import EmailConfig from "./EmailConfig.vue";
const props = withDefaults(
  defineProps<{
    defaultTab?: number;
  }>(),
  {
    defaultTab: 0,
  }
);

let tabs = [
  {
    label: "Email Accounts",
    icon: markRaw(LucideMail),
    component: markRaw(EmailConfig),
  },
  {
    label: "Branding",
    icon: markRaw(ImageUp),
    component: markRaw(Branding),
  },
];
const show: ModelRef<boolean> = defineModel();

const activeTab = ref(tabs[props.defaultTab]);
</script>
