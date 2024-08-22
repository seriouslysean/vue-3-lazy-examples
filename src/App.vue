<template>
  <div>
    <!--
    - IS in the chunk for this component
    - Lifecycle methods will begin during the parent's creation process
    -->
    <div class="component">
      <MyComponent />
    </div>

    <!--
    - NOT in the chunk for this component
    - Will async load via XHR request during the parent's creation process
    - Loading component shown while fetching
    - Lifecycle methods will run after the component is loaded
    -->
    <div class="component">
      <AsyncMyComponent />
    </div>

    <!--
    - IS in the chunk alongside this component
    - Will NOT run any lifecycle methods until toggled on via button
    -->
    <div class="component">
      <button @click="toggleMyComponentOnToggle">Toggle MyComponentOnToggle</button>
      <MyComponentOnToggle v-if="showMyComponentOnToggle" />
    </div>

    <!--
    - NOT in the chunk alongside this component
    - Will async load via XHR request when toggled on via button
    - Lifecycle methods will run after the component is loaded and toggled on
    -->
    <div class="component">
      <button @click="toggleAsyncMyComponentOnToggle">Toggle AsyncMyComponentOnToggle</button>
      <AsyncMyComponentOnToggle v-if="showAsyncMyComponentOnToggle" />
    </div>

    <!--
    - IS in the chunk for this component
    - Hydration will be delayed until the browser is idle or after a 2000ms timeout
    -->
    <div class="component">
      <LazyHydrationWrapper :when-idle="2000">
        <MyComponentWhenIdle />
      </LazyHydrationWrapper>
    </div>

    <!--
    - IS in the chunk for this component
    - Hydration will be delayed until the component becomes visible in the viewport
    -->
    <div class="component">
      <LazyHydrationWrapper :when-visible="{ rootMargin: '50px' }">
        <MyComponentWhenVisible />
      </LazyHydrationWrapper>
    </div>

    <!--
    - IS in the chunk for this component
    - Hydration will be delayed until the user interacts with the component (e.g., click or focus)
    -->
    <div class="component">
      <LazyHydrationWrapper :on-interaction="['click', 'focus']">
        <MyComponentOnInteraction />
      </LazyHydrationWrapper>
    </div>

    <!--
    - IS in the chunk for this component
    - Hydration manually triggered by a button click
    -->
    <div class="component">
      <button @click="triggerMyComponentOnTriggerHydration">Trigger Hydration</button>
      <LazyHydrationWrapper :when-triggered="hydrateMyComponentOnTrigger" @hydrated="onHydrateMyComponentOnTrigger">
        <MyComponentOnTrigger :hydrated="hydrateMyComponentOnTrigger" />
      </LazyHydrationWrapper>
    </div>
  </div>
</template>

<script>
import { defineAsyncComponent } from 'vue';
import { LazyHydrationWrapper } from 'vue3-lazy-hydration';

import MyComponent from './components/MyComponent.vue';
import LoaderComponent from './components/LoaderComponent.vue';
import MyComponentOnToggle from './components/MyComponentOnToggle.vue';
import MyComponentWhenIdle from './components/MyComponentWhenIdle.vue';
import MyComponentWhenVisible from './components/MyComponentWhenVisible.vue';
import MyComponentOnInteraction from './components/MyComponentOnInteraction.vue';
import MyComponentOnTrigger from './components/MyComponentOnTrigger.vue';

const AsyncMyComponent = defineAsyncComponent({
  loader: () => import('./components/AsyncMyComponent.vue'),
  loadingComponent: LoaderComponent,
});

const AsyncMyComponentOnToggle = defineAsyncComponent({
  loader: () => import('./components/AsyncMyComponentOnToggle.vue'),
  loadingComponent: LoaderComponent,
});

export default {
  data() {
    return {
      showMyComponentOnToggle: false,
      showAsyncMyComponentOnToggle: false,
      hydrateMyComponentOnTrigger: false,
    };
  },
  components: {
    LazyHydrationWrapper,
    MyComponent,
    AsyncMyComponent,
    MyComponentOnToggle,
    AsyncMyComponentOnToggle,
    MyComponentWhenIdle,
    MyComponentWhenVisible,
    MyComponentOnInteraction,
    MyComponentOnTrigger,
  },
  methods: {
    toggleMyComponentOnToggle() {
      this.showMyComponentOnToggle = !this.showMyComponentOnToggle;
    },
    toggleAsyncMyComponentOnToggle() {
      this.showAsyncMyComponentOnToggle = !this.showAsyncMyComponentOnToggle;
    },
    triggerMyComponentOnTriggerHydration() {
      this.hydrateMyComponentOnTrigger = true;
    },
    onHydrateMyComponentOnTrigger() {
      console.log('MyComponentOnTrigger hydrated!');
    },
  },
};
</script>

<style>
.component {
  border: 1px dotted #000;
  padding: 1em;
  margin: 1em;
}
</style>
