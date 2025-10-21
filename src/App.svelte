<script>
  import DatePicker from "./DatePicker.svelte";

  let selectedDate = $state(new Date());
</script>

<DatePicker bind:selectedDate />
