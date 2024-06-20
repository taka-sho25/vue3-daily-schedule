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
            v-for="(dayOfWeek, idx) in dayOfWeekArray"
            :key="idx"
            class="schedule-line"
          >
            <div
              ref="ruleDayOfWeekItem"
              :data-val="dayOfWeek"
              class="dayOfWeek-text"
            >
              {{ dayOfWeek }}
            </div>
            <div
              v-for="(time, i) in timeArray"
              :key="i"
              ref="ruleTimeItem"
              :data-dayOfWeek="i + 1"
              :data-time="time"
              :class="{
                schedule: true,
                pointer: !disabled,
                active: false,
              }"
              @click="toggleSchedule"
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
const toggleSchedule = () => {};
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
  padding: 10px;
  margin: 1px;
  font-size: 10px;
}
.dayOfWeek-text {
  text-align: center;
}
.schedule {
  background-color: #1d92cc;

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
