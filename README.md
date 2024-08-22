# Vue 3 Lazy Examples

This project showcases techniques for optimizing component loading and hydration in Vue 3, including async loading, lazy loading, and lazy hydration via `vue3-lazy-hydration`.

## Examples Overview

1. **MyComponent**
   - **In Chunk:** Part of the initial chunk. Lifecycle methods run during the parent's creation process.

2. **AsyncMyComponent**
   - **Async Loaded:** Not part of the initial chunk. Loads asynchronously during the parent's creation. A loading component is displayed while fetching, and lifecycle methods run after loading.

3. **MyComponentOnToggle**
   - **In Chunk:** Part of the initial chunk. Lifecycle methods do not run until the component is toggled on via a button.

4. **AsyncMyComponentOnToggle**
   - **Async Loaded:** Not part of the initial chunk. Loads asynchronously when toggled on via a button. Lifecycle methods run after loading and toggling on.

### Notes

These components do not appear to be working as expected and do not prevent hydration from what I can tell.

5. **MyComponentWhenIdle**
   - **Lazy Hydration:** Hydration is delayed until the browser is idle or after a 2000ms timeout.

6. **MyComponentWhenVisible**
   - **Lazy Hydration:** Hydration is delayed until the component becomes visible in the viewport.

7. **MyComponentOnInteraction**
   - **Lazy Hydration:** Hydration is delayed until the user interacts with the component (e.g., click or focus).

8. **MyComponentOnTrigger**
   - **Lazy Hydration:** Hydration is manually triggered by a variable.
