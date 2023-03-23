<script>
  import { getContext } from "svelte"

  const { styleable, Provider } = getContext("sdk")
  const component = getContext("component")

  export let direction
  export let align
  export let justify
  export let gap
  export let mode
  export let source
  export let delimiter
  export let startIndex
  export let iterations

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

  let _startIndex
  let _iterations
  let _slots 

  $: _startIndex = (!isNaN(startIndex)) ? Number(startIndex) : 0
  $: _iterations = (!isNaN(iterations)) ? Number(iterations) : 0
  $: _valid = !isNaN(startIndex) && !isNaN(iterations)

  $: initialize( mode, source, delimiter )

  function initialize () {
    if (!source) return

    if ( mode === "delimited" ) {
      try { 
        _slots = source.split(delimiter)
      }
      catch ( ex ) {
        console.log("error", ex)
      }
    } else if ( mode === "object" ) {
      try { 
        _slots = JSON.parse(source)
      }
      catch ( ex ) {
        console.log("error", ex)
      }
    } else {
      _slots = Array(_iterations).fill(0)
    }
  }

  function getDataContext ( index ) {
    let dataContext = {
      currentIndex : _startIndex + index,
      value: _slots[index],
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
    style:width="100%"
    style:height="100%"
    >
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
    </div>
  {:else if (!_valid)}
    <p> Oops! Something went wrong. Seems you have set invalid (Non Numerical) Properties </p>
  {/if}

</div>