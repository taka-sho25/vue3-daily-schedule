<template>
  <div
    class="daily-schedule"
    :class="{ disabled }"
  >
    <div class="select-none">
      <div class="daily-schedule-table">
        <div class="time-list">
          <div class="time-rule" />
          <div
            v-for="(time, index) in timeArray"
            :key="index"
            class="time-text"
          >
            {{ time }}
          </div>
        </div>
        <div
          id="schedule"
          ref="draggableArea"
          @mousedown="startDrag"
          @mouseup="doDrag"
        >
          <div
            v-for="(dayOfWeek, dayOfWeekIndex) in dayOfWeekArray"
            :key="dayOfWeekIndex"
            class="schedule-line"
          >
            <div
              ref="dayOfWeekItem"
              class="dayOfWeek-text"
            >
              {{ dayOfWeek }}
            </div>
            <div
              v-for="(time, i) in timeArray"
              :key="i"
              ref="timeItem"
              :data-dayOfWeek="dayOfWeekIndex"
              :data-time="time"
              class="schedule"
              :class="{
                pointer: !disabled,
                active: timetable[dayOfWeekIndex].find((el) => el === time) !== undefined,
              }"
              @click="toggleSchedule(dayOfWeekIndex, time)"
            >
              <span />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="select-schedule">
    <div>Select Schedule</div>
    <div>{{ timetable }}</div>
  </div>
</template>

<script setup lang="ts">
type Props = {
  disabled?: boolean;
};

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
});

const isDragging = ref(false);
const startDragDayOfWeek = ref(0);
const startDragTime = ref(0);
const draggableArea = ref<typeof HTMLDivElement>();
const dayOfWeekItem = ref<typeof HTMLDivElement[]>();
const timeItem = ref<typeof HTMLDivElement[]>();
const timetable = ref<Record<number, number[]>>({
  0: [],
  1: [],
  2: [],
  3: [],
  4: [],
  5: [],
  6: [],
});

const dayOfWeekArray = computed(() => {
  return ['日', '月', '火', '水', '木', '金', '土'];
});

const timeArray = computed(() => {
  return new Array(24).fill(null).map((_, i) => i);
});

const startDrag = (event: MouseEvent) => {
  if (props.disabled) {
    return;
  }

  isDragging.value = true;

  const target = event.target;
  if (!(target instanceof HTMLDivElement)) {
    return;
  }

  const { dayofweek, time } = target.dataset;
  if (!dayofweek || !time) {
    return;
  }

  startDragDayOfWeek.value = Number(dayofweek);
  startDragTime.value = Number(time);
};

const doDrag = (event: MouseEvent) => {
  if (props.disabled || !isDragging.value) {
    return;
  }

  try {
    const target = event.target;
    if (!(target instanceof HTMLDivElement)) {
      return;
    }

    const { dayofweek, time } = target.dataset;
    if (!dayofweek || !time) {
      return;
    }

    const endDragDayOfWeek = Number(dayofweek);
    const endDragTime = Number(time);
    if (startDragDayOfWeek.value === endDragDayOfWeek && startDragTime.value === endDragTime) {
      return;
    }

    for (let i = startDragDayOfWeek.value; i <= endDragDayOfWeek; i++) {
      for (let ii = startDragTime.value; ii <= endDragTime; ii++) {
        const indexDayOfWeek = timetable.value[i].findIndex(time => time === ii);
        if (indexDayOfWeek !== -1) {
          timetable.value[i].splice(indexDayOfWeek, 1);
        } else {
          timetable.value[i].push(ii);
        }
      }
    }
  } catch (e) {
    console.log(e);
  }

  isDragging.value = false;
};

const toggleSchedule = (dayOfWeekIndex: number, time: number) => {
  if (props.disabled) {
    return;
  }

  const selectedTimeIndex = timetable.value[dayOfWeekIndex].findIndex(
    (el: number) => el === time,
  );
  if (selectedTimeIndex !== -1) {
    timetable.value[dayOfWeekIndex].splice(selectedTimeIndex, 1);
  } else {
    timetable.value[dayOfWeekIndex].push(time);
  }
};
</script>

<style scoped>
.daily-schedule {
  position: relative;
}

.select-none {
  user-select: none;
}

.daily-schedule-table {
  border-collapse: collapse;
}

.time-list {
  display: flex;
  flex-wrap: wrap;

  div {
    flex-basis: 0;
    flex-grow: 1;
    max-width: 100%;
    padding: 10px 10px 6px 0;
    font-size: 10px;
  }
}
.time-rule {
  display: block;
  width: 100%;
}
.time-text {
  margin-left: -8px;
  text-align: left;
}

.schedule-line {
  display: flex;
  flex-wrap: wrap;
}

.dayOfWeek-text,
.schedule {
  flex-basis: 0;
  flex-grow: 1;
  max-width: 100%;
  padding: 8px;
  margin: 2px 0;
  font-size: 10px;
}
.dayOfWeek-text {
  text-align: center;
}
.schedule {
  background-color: var(--vds-bg);
  border-right: 1px solid var(--vds-border);

  &:last-child {
    border-right: none;
  }

  &:hover {
    background-color: var(--vds-hover);
  }

  &.active {
    background-color: var(--vds-active);

    &:hover {
      background-color: var(--vds-hover);
    }
  }
}
.pointer {
  cursor: pointer;
}

.select-schedule {
  margin-top: 20px;
}
</style>
