<script lang="ts">
  const pageCount = 5
  let selectedIdx = $state(1);


  function limit(n: number) {
    return Math.min(Math.max(0, n), pageCount-1)
  }
</script>


<style>
  circle{
    transition: cx 300ms
  }

  .mirror{
    transform: scaleX(-1);
  }

  .line-top,
  .line-bottom,
  .arrow{
    transition:d 50ms;
  }

  .arrow-btn:hover:not(:disabled) .line-top {
    d: path("M5 40 L72 40")
  }
  .arrow-btn:hover:not(:disabled) .line-bottom {
    d: path("M5 60 L72 60")
  }
  .arrow-btn:hover:not(:disabled) .arrow {
    d: path("M61 23 Q74 47 95 50 Q74 53 61 77");
  }

  .arrow-btn {
    transition: opacity 200ms;
    &:disabled {
      opacity: 0.5;
    }
  }
</style>


<!-- https://www.joshwcomeau.com/svg/friendly-introduction-to-svg/ -->

<!-- https://developer.mozilla.org/en-US/docs/Web/CSS/How_to/Layout_cookbook/Pagination -->


{#snippet arrowIcon()}
  <svg
    width=1em
    height=1em
    viewBox="0 0 100 100"
  >
    <path
      class=line-top
      d="M5 50 L95 50"
      stroke=currentColor
      stroke-width=5
      stroke-linecap=round
    />
    <path
      class=line-bottom
      d="M5 50 L95 50"
      stroke=currentColor
      stroke-width=5
      stroke-linecap=round
    />
    <path
      class=arrow
      d="M73 25 Q81 48 95 50 Q81 52 73 75"
      fill=none
      stroke=currentColor
      stroke-width=5
      stroke-linecap=round
      stroke-linejoin=round
    />
  </svg>
{/snippet}


<nav class="pagination">
  <button
    class="arrow-btn mirror"
    aria-label="previous"
    onclick={() => selectedIdx = limit(selectedIdx-1)}
    disabled={selectedIdx<=0}
  >
    {@render arrowIcon()}
  </button>


  {#each {length: pageCount} as _, i (i)}
    <button
    aria-label="page {i+1}"
    onclick={() => selectedIdx = i}
    disabled={i===selectedIdx}
    >
      <svg
        width=1em
        height=1em
        viewBox="0 0 100 100"
      >
        <defs>
          <mask id="cutout_{i}">
            <rect width=100 height=100 fill="white"/>
            <circle cx={i<selectedIdx ? 70 : 150} cy=50 r=50 fill="black"/>
            <circle cx={i>selectedIdx ? 30 : -50} cy=50 r=50 fill="black"/>
          </mask>
        </defs>
          
        <circle
          cx="50" cy="50"
          r="50"
          fill=currentColor
          mask="url(#cutout_{i})"
        />
      </svg>
    </button>
  {/each}


  <button
    class="arrow-btn"
    aria-label="next"
    onclick={() => selectedIdx = limit(selectedIdx + 1)}
    disabled={selectedIdx >= pageCount - 1}
  >
    {@render arrowIcon()}
  </button>


  <p>current index: {selectedIdx}</p>

</nav>
