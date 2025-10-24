<script lang="ts">
  import {
    format,
    subMonths,
    addMonths,
    startOfWeek,
    addDays,
    eachDayOfInterval,
    startOfMonth,
    endOfMonth,
    endOfWeek,
    isSameMonth,
    isSameDay,
  } from "date-fns";

  let { selectedDate = $bindable(new Date()) }: { selectedDate: Date } =
    $props();

  let currentMonth = $state(new Date());
  let _eachDayOfInterval = $derived(
    eachDayOfInterval({
      start: startOfWeek(startOfMonth(currentMonth)),
      end: endOfWeek(endOfMonth(currentMonth)),
    })
  );

  const _startOfWeek = startOfWeek(new Date());
  const daysArray = Array.from({ length: 7 }, (_, i) => i + 1);

  const prevMonth = () => {
    currentMonth = subMonths(currentMonth, 1);
  };

  const nextMonth = () => {
    currentMonth = addMonths(currentMonth, 1);
  };

  const selectDate = (day: Date) => {
    selectedDate = day;
  };

  const renderDateBgColor = (day: Date) => {
    return isSameDay(day, selectedDate);
  };

  const renderDateColor = (day: Date) => {
    return isSameMonth(day, currentMonth);
  };
</script>

<div class="wrapper">
  <h1 class="title">Svelte Date Picker</h1>

  <div class="date-picker-container">
    <div class="details-container">
      <p class="year-text">{format(selectedDate, "yyyy")}</p>
      <p class="day-text">{format(selectedDate, "EEEE, MMMM do")}</p>
    </div>

    <div class="date-picker-inner-wrapper">
      <div class="date-picker-header">
        <button class="arrow-container" onclick={prevMonth}> &#8592; </button>
        <p>{format(currentMonth, "MMMM yyyy")}</p>
        <button class="arrow-container" onclick={nextMonth}> &#8594; </button>
      </div>

      <div class="date-picker-days">
        {#each daysArray as _, i (i)}
          <p>
            {format(addDays(_startOfWeek, i), "EEE")}
          </p>
        {/each}
      </div>

      <div class="date-picker-dates">
        {#each _eachDayOfInterval as day (day)}
          <button
            class:date-selected={renderDateBgColor(day)}
            class:date-color={renderDateColor(day)}
            onclick={() => {
              if (isSameMonth(day, currentMonth)) {
                selectDate(day);
              }
            }}
          >
            {format(day, "d")}
          </button>
        {/each}
      </div>
    </div>
  </div>
</div>
