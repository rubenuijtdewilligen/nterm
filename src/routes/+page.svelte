<script>
  let max;
  let norm;
  let points;
  let score;
  let emoji;
  let visibleElement;

  $: max = 0;
  $: norm = 1.0;
  $: points = 0;
  $: score = 0;
  $: emoji = '';
  $: visibleElement = 'input';

  function calculateGrade(max, norm, points) {
    let defaultFormula = 9.0 * (points / max) + norm;

    let border1 = 1.0 + points * (9.0 / max) * 2;
    let border2 = 1.0 + points * (9.0 / max) * 0.5;
    let border3 = 10 - (max - points) * (9.0 / max) * 2;
    let border4 = 10 - (max - points) * (9.0 / max) * 0.5;

    if (norm == 1) score = defaultFormula;

    if (norm > 1) {
      if (points <= max / 3) score = Math.min(defaultFormula, border1);
      if (points > max / 3) score = Math.min(defaultFormula, border4);
    }

    if (norm < 1) {
      if (points <= (2 * max) / 3) score = Math.max(defaultFormula, border2);
      if (points > (2 * max) / 3) score = Math.max(defaultFormula, border3);
    }

    score = Math.round(score * 10) / 10;

    if (score < 3) {
      emoji = '😭';
    } else if (score < 5.5) {
      emoji = '😬';
    } else if (score < 6.5) {
      emoji = '😄';
    } else if (score < 8) {
      emoji = '😁';
    } else if (score <= 10) {
      emoji = '🥳';
    } else {
      emoji = '❌';
    }

    visibleElement = 'output';
  }
</script>

<div class="flex h-full w-full flex-col items-center">
  <h2 class="mt-2 text-center text-3xl font-bold tracking-tight text-base-content">
    nterm.rbnu.nl
  </h2>

  <p class="mt-1 text-center">Gratis tool om je CE-cijfer in te schatten</p>

  <div class="divider"></div>

  {#if visibleElement === 'input'}
    <form class="flex w-full flex-col items-center" on:submit={calculateGrade(max, norm, points)}>
      <div class="form-control mb-2 w-full max-w-lg">
        <label for="max" class="label pb-1 font-medium">
          <span class="label-text">Geschatte n-term (0,0 - 2,5)</span>
        </label>
        <input
          class="input input-bordered w-full max-w-lg"
          type="number"
          id="norm"
          name="norm"
          min="0"
          step="0.1"
          max="2.5"
          required
          bind:value={norm}
        />
      </div>

      <div class="form-control mb-2 w-full max-w-lg">
        <label for="max" class="label pb-1 font-medium">
          <span class="label-text">Maximaal aantal punten</span>
        </label>
        <input
          class="input input-bordered w-full max-w-lg"
          type="number"
          id="max"
          name="max"
          min="0"
          required
          bind:value={max}
        />
      </div>

      <div class="form-control mb-2 w-full max-w-lg">
        <label for="max" class="label pb-1 font-medium">
          <span class="label-text">Behaalde punten</span>
        </label>
        <input
          class="input input-bordered w-full max-w-lg"
          type="number"
          id="max"
          name="max"
          min="0"
          {max}
          required
          bind:value={points}
        />
      </div>

      <div class="w-full max-w-lg pt-2">
        <button type="submit" class="btn btn-primary w-full">Berekenen</button>
      </div>
    </form>
  {:else if visibleElement === 'output'}
    <div class="flex flex-col items-center">
      <h1 class="mb-6 text-7xl">{emoji}</h1>
      <p>Je geschatte cijfer is een</p>
      <h2 class="pb-3 pt-2 text-3xl font-bold">{score}</h2>

      <button
        class="cursor-pointer underline"
        on:click={() => {
          score = 0;
          visibleElement = 'input';
        }}
      >
        Nog een cijfer berekenen?
      </button>
    </div>
  {/if}
</div>
