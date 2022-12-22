<script>
  import { nanoid } from "nanoid/nanoid";
  import { inview } from "svelte-inview";

  const id = nanoid();

  let isInView;

  export let value;
  export let initial = 0;
  export let duration = 100;
  export let step = 1;
  export let roundto = 1;
  export let format = true;
	export let ease = 0.5;
	
	if (ease > 1) ease = 1;
	
	let cur = 0;
	let thisEase = 0;

  function formatNumber(input) {
    if (format) {
      return Math.round(input).toLocaleString();
    }
    return input;
  }

  const counterResult = [];
  const timers = [];

  const max = parseInt(value);
  while (duration / ((max - initial) / step) < 2) {
    step++;
  }

  counterResult[id] = initial;
  timers[id] = setInterval(() => {
    if (isInView) {
      if (Math.round(cur / roundto) * roundto < max) {
				thisEase = (max - cur) / ((1 - ease) * 100);
        counterResult[id] += thisEase;
				cur = counterResult[id];
      } else {
        clearInterval(timers[id]);
        counterResult[id] = Math.round(max / roundto) * roundto;
      }
    }
  }, duration / ((max - initial) / step));
</script>

<span
  use:inview
  on:change={(event) => {
    const { inView } = event.detail;
    isInView = inView;
  }}>{formatNumber(counterResult[id])}</span
>