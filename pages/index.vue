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
              ref="ruleDayOfWeekItem"
              class="dayOfWeek-text"
            >
              {{ dayOfWeek }}
            </div>
            <div
              v-for="(time, i) in timeArray"
              :key="i"
              ref="ruleTimeItem"
              :data-dayOfWeek="dayOfWeekIndex"
              :data-time="time"
              :class="{
                schedule: true,
                pointer: !disabled,
                active: false,
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

withDefaults(defineProps<Props>(), {
  disabled: false,
});

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

const startDrag = () => {};
const doDrag = () => {};
const toggleSchedule = (dayOfWeekIndex: number, time: number) => {};
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
.schedule-line .schedule:nth-child(2) {
  border-radius: 50% 0 0 50%;
}
.schedule-line .schedule:nth-child(25) {
  border-radius: 0 50% 50% 0;
}
.dayOfWeek-text,
.schedule {
  flex-basis: 0;
  flex-grow: 1;
  max-width: 100%;
  padding: 8px;
  margin: 4px 0;
  font-size: 10px;
}
.dayOfWeek-text {
  text-align: center;
}
.schedule {
  background-color: #1d92cc;
  border-right: 1px solid #005a86;

  &:last-child {
    border-right: none;
  }

  &:hover {
    background-color: #a5d3ff;
  }

  &.active {
    background-color: #20a0e0;

    &:hover {
      background-color: #20a0e0;
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
