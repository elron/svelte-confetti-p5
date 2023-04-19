<script>
  import ConfettiRain from "../lib/ConfettiRain.svelte";
</script>

Hello this is my confetti!
<!-- <ConfettiRain /> -->
<!-- <ConfettiRain
  loop
  colors={["#fbfefd", "#00186a", "#003afb", "#7aa8f7"]}
  amount={150}
  weight={100}
  minSize={5}
  maxSize={15}
  spacing={10}
  flip={3}
  rotate={3}
  power={10}
/> -->
<!-- <ConfettiRain
  colors={["#fbfefd", "#00186a", "#003afb", "#7aa8f7"]}
  amount={50}
  weight={200}
  minSize={20}
  maxSize={50}
  spacing={3}
  flip={3}
  rotate={3}
  power={20}
/> -->
<ConfettiRain
  colors={["#e84b5e", "#f9db05", "#fefffc", "#ffbe24"]}
  shapes="squares"
  minSize={5}
  maxSize={25}
  spacing={15}
  amount={80}
  flip={2}
  power={5}
  weight={200}
/>
