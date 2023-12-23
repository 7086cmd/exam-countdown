<template>
  <div class="container">
    <div class="card">
      <h2>距离 2023 学年高一期末考试还剩</h2>
      <h1>{{ countdown }}</h1>
      <p
        style="text-align: left; padding-left: 2rem"
        v-for="(exam, idx) in exams
          .filter((x) => x.display && dayjs(x.start).diff(currentDate, 'minute') > 0)
          .filter((_, idx) => idx <= 3)"
        :key="exam.subject"
      >
        {{ orderlist[idx] }}距离{{ exam.subject }}考试还剩
        {{ dayjs(exam.start).diff(currentDate, 'day') }} 天
        {{ dayjs(exam.start).diff(currentDate, 'hour') % 24 }} 小时
        {{ dayjs(exam.start).diff(currentDate, 'minute') % 60 }} 分钟
        <span>（{{ dayjs(exam.start).format('YYYY-MM-DD HH:mm:ss') }}——{{ dayjs(exam.end).format('HH:mm:ss') }}）</span>
      </p>
      <p
        v-if="
          exams.filter((x) => x.display && dayjs(x.start).diff(currentDate, 'minute') > 0).length >=
          3
        "
      >
        ……
      </p>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import dayjs from 'dayjs'

export default {
  setup() {
    // 目标日期
    const targetDate = dayjs('2024-01-09')
    // 当前日期
    const currentDate = ref(dayjs())
    // 倒计时
    const countdown = computed(() => {
      // 计算时间差
      const diff = targetDate.diff(currentDate.value)
      // 如果时间差小于等于零，返回结束
      if (diff <= 0) {
        return '结束'
      }
      // 否则，转换为天，小时，分钟
      const days = Math.floor(diff / (1000 * 60 * 60 * 24))
      const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
      const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
      // 返回格式化的字符串
      return `${days} 日 ${hours} 小时 ${minutes} 分钟`
    })
    // 每隔一秒更新当前日期
    setInterval(() => {
      currentDate.value = dayjs()
    }, 600)
    const exams = [
      { subject: '语文', start: '2024-01-09 08:30:00', end: '2024-01-09 11:00:00', display: true },
      { subject: '生物', start: '2024-01-09 13:30:00', end: '2024-01-09 15:00:00', display: false },
      { subject: '地理', start: '2024-01-09 16:00:00', end: '2024-01-09 17:00:00', display: false },
      { subject: '数学', start: '2024-01-10 08:00:00', end: '2024-01-11 10:00:00', display: true },
      { subject: '技术', start: '2024-01-10 10:30:00', end: '2024-01-10 12:00:00', display: true },
      { subject: '物理', start: '2024-01-10 13:30:00', end: '2024-01-10 15:00:00', display: true },
      { subject: '政治', start: '2024-01-10 16:00:00', end: '2024-01-10 17:00:00', display: false },
      { subject: '化学', start: '2024-01-11 08:30:00', end: '2024-01-11 10:00:00', display: true },
      { subject: '历史', start: '2024-01-11 11:00:00', end: '2024-01-11 12:00:00', display: false },
      { subject: '英语', start: '2024-01-11 14:00:00', end: '2024-01-11 16:00:00', display: true }
    ]
    // 返回响应式数据
    return {
      countdown,
      exams,
      dayjs,
      orderlist: ['①', '②', '③', '④', '⑤', '⑥', '⑦', '⑧', '⑨', '⑩']
    }
  }
}
</script>

<style>
body {
  background-image: url('@/assets/focalors.jpg');
  overflow-x: hidden;
  background-size: contain;
  overflow: hidden;
  background-repeat: no-repeat;
  background-position: center;
  background-attachment: fixed;
  font-family: 'Focalors';
  font-weight: normal;
}

h1 {
  font-weight: normal;
}

@font-face {
  font-family: 'Focalors';
  src: url('@/assets/font.ttf');
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.card {
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 0.5rem;
  padding: 4rem;
  text-align: center;
}

h1 {
  font-size: 96px;
  margin: -1rem 0 0 0;
}

h2 {
  font-size: 48px;
  margin: 0;
  padding-bottom: 1rem;
}

p {
  font-size: 36px;
  margin: 0;
}

span {
  font-size: 18px;
}
</style>
