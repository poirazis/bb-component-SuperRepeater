<script>
  import { getContext } from "svelte"

  const { styleable, Provider } = getContext("sdk")
  const component = getContext("component")

  export let direction
  export let align
  export let justify
  export let gap

  //  'start' | 'center' | 'end' | 'stretch'
  const alignMap = {
    start: 'flex-start',
    center: 'center',
    end: 'flex-end',
    stretch: 'stretch',
  };
  //  'start' | 'center' | 'end' | 'around' | 'between' | 'evenly'
  const justifyMap = {
    start: 'flex-start',
    center: 'center',
    end: 'flex-end',
    around: 'space-around',
    between: 'space-between',
    evenly: 'space-evenly',
  };

  export let startIndex
  export let iterations

  let _startIndex
  let _iterations

  $: _startIndex = (!isNaN(startIndex)) ? Number(startIndex) : 0
  $: _iterations = (!isNaN(iterations)) ? Number(iterations) : 0
  $: _slots = Array(_iterations).fill(0)
  $: _valid = !isNaN(startIndex) && !isNaN(iterations)

  function getDataContext ( index ) {
    let dataContext = {
      currentIndex : _startIndex + index,
      isFirst : index === 0,
      isLast : index === (_iterations - 1)
    }

    return dataContext
  }

</script>

<div 
  use:styleable={$component.styles}>
  {#if _slots?.length > 0}
  <div
  {...$$restProps}
  class={$$restProps.class}
  style:display="flex"
  style:flex-direction={direction}
  style:align-items={alignMap[align]}
  style:justify-content={justifyMap[justify]}
  style:gap={gap}
  >
  {#key _startIndex}
    {#each _slots as _slot, index}
      <Provider data={getDataContext(index)}>
          <slot />
      </Provider>
    {/each}
  {/key}
  </div>
  {:else if (!_valid)}
    <p> Invalid Properties </p>
  {:else}
    <p> You have iterations set at 0. The component will not render at runtime </p>
  {/if}
</div>
