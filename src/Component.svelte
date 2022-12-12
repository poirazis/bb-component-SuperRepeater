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
  $: console.log ( _startIndex )
  $: console.log ( _valid )

  function getDataContext ( index ) {
    let dataContext = {
      currentIndex : _startIndex + index,
      isFirst : index === 0,
      isLast : index === (_iterations - 1)
    }
    return dataContext
  }

</script>

<div use:styleable={$component.styles}>

  {#if _slots?.length > 0 && _valid}
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
      {#if $component.empty}
        <div class="blankState">
          <h2> Welcome to the Super Repeater Component </h2>
          <p> Add some Components to the Super Repeater to get Started ! </p>
          <p> Then set you Starting Index and Iterations properties. </p>
        </div>
      {:else}
        {#each _slots as _slot, index}
          <Provider data={getDataContext(index)}>
              <slot />
          </Provider>
        {/each}
      {/if}
    {/key}
    </div>
  {:else if (!_valid)}
    <p> Oops! Something went wrong. Seems you have set invalid (Non Numerical) Properties </p>
  {/if}

</div>