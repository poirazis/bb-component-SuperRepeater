<script>
  import { getContext } from "svelte";
  import { SuperTabs } from "@poirazis/supercomponents-shared";
  import fsm from "svelte-fsm";

  const {
    styleable,
    builderStore,
    processStringSync,
    Provider,
    ContextScopes,
  } = getContext("sdk");

  const component = getContext("component");

  export let dataprovider;
  export let sourceArray;
  export let sourceJSON;
  export let bound = "dataprovider";

  export let mode = "container";
  export let direction = "column";
  export let hAlign = "stretch";
  export let vAlign = "flex-start";
  export let gap = "1";
  export let wrap = false;

  export let tabsPosition = "top";
  export let tabText = "Tab {{ index }}";
  export let theme = "buttons";
  export let buttonsAlignment = "flex-start";
  export let tabsIconsOnly = false;
  export let tabsAlignment = "center";
  export let list_title = "Items";
  export let list_icon = "ri-layout-list-line";
  export let activeTab = 0;

  export let gridColumns = 3;
  export let gridRows = 3;

  // Events as Parent
  export let onTabChange;

  let selectedTab = undefined;

  let cssVariables = {};

  // The State machine that handles layout modes
  const state = fsm(mode, {
    "*": {
      refresh() {},
      synchProperties() {
        this.refresh();
        return mode;
      },
    },
    container: {
      _enter() {
        this.refresh();
      },
      refresh() {
        cssVariables = {
          "flex-direction": direction,
          "flex-wrap": wrap ? "wrap" : "nowrap",
          "justify-content": direction == "row" ? hAlign : vAlign,
          "align-items": direction == "row" ? vAlign : hAlign,
          "align-content": wrap ? (direction == "row" ? vAlign : hAlign) : null,
          gap: gap + "rem",
          "--container-flex-mode":
            (direction == "row" && hAlign == "stretch") ||
            (direction == "column" && vAlign == "stretch")
              ? "1"
              : null,
        };
      },
    },
    grid: {
      _enter() {
        this.refresh();
      },
      refresh() {
        cssVariables = {
          display: "grid",
          "justify-items": hAlign,
          "align-items": vAlign,
          "--grid-columns": gridColumns,
          "--grid-rows": gridRows > 1 ? gridRows : "auto",
          "--grid-column-gap": gap + "rem",
          "--grid-row-gap": gap + "rem",
        };
      },
    },
    tabs: {
      _enter() {
        this.refresh();
      },
      refresh() {
        cssVariables = {
          display: "flex",
          "flex-direction": tabsPosition == "left" ? "row" : "column",
          "align-items": "stretch",
        };
      },
      selectTab(tabId) {
        selectedTab = tabId;
        onTabChange?.({ tabTitle: "tab" });
      },
    },
    list: {
      _enter() {
        this.refresh();
      },
      refresh() {
        cssVariables = {
          display: "flex",
          "flex-direction": "row",
          "align-items": "stretch",
        };
      },
      selectTab(tabId) {
        selectedTab = tabId;
        onTabChange?.({ tabTitle: "tab" });
      },
    },
  });

  let slots = [];

  $: inBuilder = $builderStore.inBuilder;

  $: slots =
    bound == "array"
      ? safeParse(sourceArray, false)
      : bound == "json"
        ? safeParse(sourceJSON, true)
        : bound == "dataprovider"
          ? dataprovider?.rows || []
          : [];

  $: tabContainers = Array.isArray(slots)
    ? slots.map((row, idx) => ({
        id: idx,
        title: processStringSync(tabText, {
          index: idx,
          [$component.id]: {
            ...row,
            index: idx,
            value: bound == "array" ? row : bound == "json" ? row?.value : row,
          },
        }),
      }))
    : [];

  // Update on property changes
  $: state.synchProperties(
    bound,
    mode,
    hAlign,
    vAlign,
    direction,
    gap,
    gridColumns,
    gridRows,
    wrap,
    tabsPosition
  );

  function safeParse(str, isJSON = false) {
    let parsed;

    if (!str) return [];

    // Check if already an array or object
    if (Array.isArray(str)) return str;
    if (typeof str === "object" && str !== null) {
      // If it's already an object
      if (isJSON) {
        return Object.keys(str).map((key) => ({
          key,
          value: str[key],
        }));
      }
      // For array mode with object
      return Object.keys(str).map((key) => str[key]);
    }

    // Try to parse as string
    try {
      parsed = JSON.parse(str);

      // If parsing object keys (JSON mode)
      if (isJSON && typeof parsed === "object" && !Array.isArray(parsed)) {
        return Object.keys(parsed).map((key) => ({
          key,
          value: parsed[key],
        }));
      }

      // For array mode
      if (typeof parsed === "object") {
        if (Array.isArray(parsed)) return parsed;
        const arrayOfObjects = Object.keys(parsed).map((key) => parsed[key]);
        return arrayOfObjects;
      }
    } catch (error) {
      parsed = [];
    }

    return parsed;
  }

  // Update selected tab when activeTab changes
  $: if ((mode == "tabs" || mode == "list") && selectedTab === undefined) {
    const tabIndex = Number(activeTab);
    if (tabIndex >= 0 && tabIndex < slots.length) {
      selectedTab = tabIndex;
      state.selectTab(tabIndex);
    }
  }

  // Append Component Styles
  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      ...cssVariables,
    },
  };

  $: visible = slots.length > 0 || inBuilder;
</script>

{#if visible}
  <div
    class:super-repeater={$state == "container"}
    class:super-grid={$state == "grid"}
    class:repeater-tabs={$state == "tabs"}
    class:list={$state == "list"}
    class:repeater-tab-item={$state == "tabs" || $state == "list"}
    use:styleable={$component.styles}
  >
    {#if bound && slots.length > 0}
      {#if mode == "tabs" || mode == "list"}
        <SuperTabs
          containers={tabContainers}
          {selectedTab}
          {direction}
          theme={mode == "list" ? "list" : theme}
          tabsPosition={mode == "list" ? "left" : tabsPosition}
          {tabsAlignment}
          {buttonsAlignment}
          {tabsIconsOnly}
          {list_icon}
          {list_title}
          on:change={(e) => {
            selectedTab = e.detail.id;
            onTabChange?.({ tabTitle: "tab" });
          }}
        />
      {/if}

      {#if mode == "tabs" || mode == "list"}
        {#if selectedTab !== undefined && slots[selectedTab]}
          <div class="repeater-tab-content">
            <Provider
              data={bound == "array"
                ? { value: slots[selectedTab], index: selectedTab }
                : bound == "json"
                  ? {
                      index: selectedTab,
                      key: slots[selectedTab]?.key,
                      value: slots[selectedTab]?.value,
                    }
                  : {
                      ...slots[selectedTab],
                      index: selectedTab,
                      value: slots[selectedTab],
                    }}
              scope={ContextScopes.Local}
            >
              <slot />
            </Provider>
          </div>
        {/if}
      {:else}
        {#each slots as row, idx (idx)}
          <div class="repeater-item">
            <Provider
              data={bound == "array"
                ? { value: row, index: idx }
                : { ...row, index: idx, _row: row }}
              scope={ContextScopes.Local}
            >
              <slot />
            </Provider>
          </div>
        {/each}
      {/if}
    {:else if inBuilder}
      <slot />
    {/if}
  </div>
{/if}

<style>
  :global(.super-grid > .component > *) {
    overflow: hidden;
  }

  .super-repeater {
    display: flex;
    position: relative;
    overflow: hidden;
  }

  .super-grid {
    display: grid;
    position: relative;
    grid-template-columns: repeat(var(--grid-columns), 1fr);
    grid-template-rows: repeat(var(--grid-rows), 1fr);
    column-gap: var(--grid-column-gap);
    row-gap: var(--grid-row-gap);
  }

  .repeater-tabs {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    justify-content: stretch;
    width: 100%;
    height: 100%;
  }

  .repeater-tab-item {
    flex: 1 1 auto;
  }

  .repeater-tab-content {
    display: flex;
    flex-direction: column;
    flex: 1 1 auto;
    overflow: auto;
    min-height: 0;
  }
</style>
