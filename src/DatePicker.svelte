<script lang="ts">
  import {
    addDays,
    addMonths,
    eachDayOfInterval,
    endOfMonth,
    endOfWeek,
    format,
    isSameDay,
    isSameMonth,
    startOfMonth,
    startOfWeek,
    subMonths,
  } from "date-fns";

  let { selectedDate = $bindable(new Date()) }: { selectedDate: Date } =
    $props();

  let currentMonth = $state(new Date());
  let _startOfWeek = startOfWeek(new Date());
  let daysArray = Array.from({ length: 7 }, (_, i) => i + 1);
  let _eachDayOfInterval = $derived(
    eachDayOfInterval({
      start: startOfWeek(startOfMonth(currentMonth)),
      end: endOfWeek(endOfMonth(currentMonth)),
    })
  );

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

<main class="wrapper">
  <h1 class="title">Svelte Date Picker</h1>

  <div class="date-picker-container">
    <div class="details-container">
      <p class="year-text">{format(selectedDate, "yyyy")}</p>
      <p class="day-text">{format(selectedDate, "EEEE MMMM, do")}</p>
    </div>

    <div class="date-picker-inner-wrapper">
      <div class="date-picker-header">
        <button class="arrow-container" onclick={prevMonth}> &#8592 </button>
        <p>{format(currentMonth, "MMMM yyyy")}</p>
        <button class="arrow-container" onclick={nextMonth}> &#8594; </button>
      </div>
    </div>

    <div class="date-picker-days">
      {#each daysArray as _, i (i)}
        <p>{format(addDays(_startOfWeek, i), "EEE")}</p>
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
</main>

<style>
  .wrapper {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 10px;
  }

  .title {
    color: #ffffff;
    font-weight: 700;
    font-size: 32px;
  }

  .date-picker-container {
    border: 1px solid rgb(102, 102, 102);
    border-radius: 18px;
    height: 540px;
  }

  .details-container {
    padding: 20px;
    border-bottom: 1px solid rgb(102, 102, 102);
  }

  .year-text {
    color: rgb(205, 201, 201);
    margin-bottom: 4px;
    font-weight: 500;
  }

  .day-text {
    color: #ffffff;
    font-size: 16px;
    font-weight: 500;
  }

  .date-picker-inner-wrapper {
    padding: 20px;
  }

  .date-picker-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 20px;
  }

  .date-picker-header p {
    color: #ffffff;
    font-size: 16px;
    font-weight: 500;
  }

  .arrow-container {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: transparent;
    border-radius: 8px;
    width: 50px;
    height: 30px;
    outline: none;
    border: 1px solid white;
    color: white;
    font-size: 18px;
    padding: 0;
  }

  .date-picker-days {
    display: grid;
    grid-template-columns: repeat(7, 50px);
    gap: 3px;
    text-align: center;
    width: fit-content;
    margin: 20px auto;
  }

  .date-picker-days p {
    color: #ffffff;
    font-size: 16px;
    font-weight: 500;
  }

  .date-picker-dates {
    display: grid;
    grid-template-columns: repeat(7, 50px);
    gap: 3px;
    text-align: center;
    width: fit-content;
    margin: 0 auto;
  }

  .date-picker-dates button {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50px;
    font-weight: 400;
    font-size: 16px;
    background-color: transparent;
    color: #7e7b7b;
    text-align: center;
    border: none;
    border-radius: 9999px;
    padding: 10px 0;
  }

  .date-selected {
    background-color: #00a15d !important;
  }
  .date-color {
    color: #ffffff !important;
    font-weight: 600 !important;
  }
</style>
