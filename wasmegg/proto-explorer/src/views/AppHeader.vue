<template>
  <header class="shadow bg-white border-b border-gray-200">
    <h1 class="mx-4 my-4 text-center text-lg leading-6 font-medium text-gray-900">
      Proto explorer
    </h1>
    <div class="max-w-ultrawide w-full mx-auto px-4 xl:px-0 space-y-2">
      <div class="space-y-1">
        <p class="text-xs text-red-500">
          Be forewarned that this tool puts great power into the hands of even non-technical folk.
          As such it can facilitate the infliction of chaos and damage in the wrong hands. Unless
          you want to see this tool taken down by the author,
          <strong
            >please do not discuss or share the URL in the public or where naughty kiddies are
            around</strong
          >. If you're not sure whether there are naughty kiddies lurking in your audience, assume
          there are.
        </p>
        <p class="text-sm font-medium text-gray-700">
          App version:
          <code class="text-xs font-mono">{{ appVersion }}</code>
          <span class="font-normal">
            (<a
              href="https://github.com/fanaticscripter/EggContractor/tree/master/misc/protobuf"
              target="_blank"
              class="hover:text-gray-500 border-b border-gray-500 border-dashed"
              >protobuf definitions</a
            >,
            <router-link
              :to="{ name: 'doc' }"
              target="_blank"
              class="hover:text-gray-500 border-b border-gray-500 border-dashed"
              >generated documentation</router-link
            >)
          </span>
        </p>
      </div>

      <div class="mt-2">
        <div class="sm:hidden mb-2">
          <label for="tabs" class="sr-only">Select a tab</label>
          <select
            id="tabs"
            v-model="selectedRoute"
            name="tabs"
            class="block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm rounded-md"
          >
            <option v-for="tab in tabs" :key="tab.name" :value="tab.name">{{ tab.title }}</option>
          </select>
        </div>
        <div class="hidden sm:block">
          <div class="pb-px -mb-px overflow-x-scroll">
            <nav class="-mb-px flex space-x-6" aria-label="Tabs">
              <router-link
                v-for="tab in tabs"
                :key="tab.name"
                :to="{ name: tab.name }"
                href="#"
                class="whitespace-nowrap py-2 border-b-2 font-medium text-sm"
                :class="
                  tab.name === route
                    ? 'border-blue-500 text-blue-600'
                    : 'border-transparent text-gray-700 hover:text-gray-900 hover:border-gray-300'
                "
                aria-current="page"
              >
                <component :is="tab.title.includes('/') ? 'code' : 'span'">
                  {{ tab.title }}
                </component>
              </router-link>
            </nav>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent, ref, toRefs, watch } from 'vue';
import { useRouter } from 'vue-router';

import { APP_VERSION } from 'lib';

export default defineComponent({
  props: {
    route: {
      type: String,
      required: true,
    },
  },

  setup(props) {
    const router = useRouter();
    const { route } = toRefs(props);

    const appVersion = APP_VERSION;
    const tabs = [
      {
        name: 'arbitrary_payload',
        title: 'Arbitrary payload',
      },
      {
        name: 'first_contact',
        title: '/first_contact',
      },
      {
        name: 'coop_status',
        title: '/coop_status',
      },
      {
        name: 'coop_status_basic',
        title: '/coop_status_basic',
      },
      {
        name: 'contracts_archive',
        title: 'ei_ctx/get_contracts_archive',
      },
      {
        name: 'get_periodicals',
        title: '/get_periodicals',
      },
      {
        name: 'get_config',
        title: '/get_config',
      },
      {
        name: 'afx/config',
        title: 'afx/config',
      },
      {
        name: 'afx/complete_mission',
        title: 'afx/complete_mission',
      },
    ];
    const selectedRoute = ref(route.value);

    watch(selectedRoute, () => router.push({ name: selectedRoute.value }));

    return {
      appVersion,
      tabs,
      selectedRoute,
    };
  },
});
</script>
