<script>
  import {Motion, useAnimation} from "svelte-motion";
  import {page} from '$app/stores'
  const variants = {
      visible: { opacity: 1, x: 0 },
      hidden: { opacity: 0, x: -500 },
  };

  let i = 1;
  $: t = [0, 360][i];
  $: v = ["hidden", "visible"][i];
  const again = () => {
      i = i > 0 ? 0 : 1;
  };


</script>
do you want svelte-motion?
<a href="https://svelte-motion.gradientdescent.de/animation">click this</a>
<div class="flex gap-10 p-10">
  <!--Basic Animation-->
  <Motion animate={{ rotate: t }} transition={{ duration: 2 }} let:motion>
      <div use:motion class="h-16 w-16 bg-black"/>
  </Motion>

  <!--Transition prop-->
  <Motion
      animate={{ rotate: t }}
      transition={{ ease: 'easeOut', duration: 2 }}
      let:motion>
      <div use:motion class="h-16 w-16 bg-black"/>
  </Motion>

  <!--Keyframes-->
  <Motion
      animate={{ rotate: [t - 360, t, 45] }}
      transition={{ duration: 3, times: [0, 0.2, 1] }}
      let:motion>
      <div use:motion class="h-16 w-16 bg-black"/>
  </Motion>

  <!--Variants-->
  <Motion initial="hidden" animate={v} {variants} let:motion>
      <div use:motion class="h-16 w-16 bg-black"/>
  </Motion>

  <button class="btn" on:click={again}>again</button>
</div>
other svelte-motion?

<a href="/{$page.stuff.user}/ex/motion/1">1</a>
<a href="/{$page.stuff.user}/ex/motion/2">2</a>
<a href="/{$page.stuff.user}/ex/motion/3">3</a>
<a href="/{$page.stuff.user}/ex/motion/4">4</a>