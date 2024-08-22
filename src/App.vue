<template>
  <div>
    <!--
    - IS in the chunk for this component
    - Lifecycle methods will begin during the parent's creation process
    -->
    <MyComponent />

    <!--
    - NOT in the chunk for this component
    - Will async load via XHR request during the parent's creation process
    - Loading component shown while fetching
    - Lifecycle methods will run after the component is loaded
    -->
    <AsyncMyComponent />

    <!--
    - IS in the chunk alongside this component
    - Will NOT run any lifecycle methods until toggled on via button
    -->
    <MyComponentOnToggle v-if="showThirdComponent" />
    <button @click="toggleThirdComponent">Toggle MyComponentOnToggle</button>

    <!--
    - NOT in the chunk alongside this component
    - Will async load via XHR request when toggled on via button
    - Lifecycle methods will run after the component is loaded and toggled on
    -->
    <AsyncMyComponentOnToggle v-if="showFourthComponent" />
    <button @click="toggleFourthComponent">
      Toggle AsyncMyComponentOnToggle
    </button>

    <!-- 
    - IS in the chunk for this component
    - Hydration will be delayed until the browser is idle or after a 2000ms timeout
    -->
    <LazyHydrationWrapper :when-idle="2000">
      <MyComponentWhenIdle />
    </LazyHydrationWrapper>

    <!-- 
    - IS in the chunk for this component
    - Hydration will be delayed until the component becomes visible in the viewport
    -->
    <LazyHydrationWrapper :when-visible="{ rootMargin: '50px' }">
      <MyComponentWhenVisible />
    </LazyHydrationWrapper>

    <!-- 
    - IS in the chunk for this component
    - Hydration will be delayed until the user interacts with the component (e.g., click or focus)
    -->
    <LazyHydrationWrapper :on-interaction="['click', 'focus']">
      <MyComponentOnInteraction />
    </LazyHydrationWrapper>

    <!-- 
    - IS in the chunk for this component
    - Hydration manually triggered by a button click
    -->
    <button @click="triggerHydration">Trigger Hydration</button>
    <LazyHydrationWrapper :when-triggered="hydrationTriggered">
      <MyComponentOnTrigger />
    </LazyHydrationWrapper>
  </div>
</template>

<script>
import { LazyHydrationWrapper } from 'vue3-lazy-hydration';
import MyComponent from './components/MyComponent.vue';
import LoaderComponent from './components/LoaderComponent.vue';

const AsyncMyComponent = defineAsyncComponent({
  loader: () => import('./components/AsyncMyComponent.vue'),
  loadingComponent: LoaderComponent,
});

const MyComponentOnToggle = import('./components/MyComponentOnToggle.vue');
const AsyncMyComponentOnToggle = defineAsyncComponent({
  loader: () => import('./components/AsyncMyComponentOnToggle.vue'),
  loadingComponent: LoaderComponent,
});

// New components for lazy hydration examples
import MyComponentWhenIdle from './components/MyComponentWhenIdle.vue';
import MyComponentWhenVisible from './components/MyComponentWhenVisible.vue';
import MyComponentOnInteraction from './components/MyComponentOnInteraction.vue';
import MyComponentOnTrigger from './components/MyComponentOnTrigger.vue';

export default {
  data() {
    return {
      showThirdComponent: false,
      showFourthComponent: false,
      hydrationTriggered: false,
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
    toggleThirdComponent() {
      this.showThirdComponent = !this.showThirdComponent;
    },
    toggleFourthComponent() {
      this.showFourthComponent = !this.showFourthComponent;
    },
    triggerHydration() {
      this.hydrationTriggered = true;
    },
  },
};
</script>
