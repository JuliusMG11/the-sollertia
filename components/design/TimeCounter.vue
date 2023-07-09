<script setup>
 const targetDate = new Date('2023-07-31'); // Replace with your target date

    const currentTime = ref(new Date());

    // Calculate the remaining time
    const remainingTime = computed(() => {
      const timeDifference = targetDate.getTime() - currentTime.value.getTime();

      // Calculate days, hours, minutes, and seconds
      const days = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
      const hours = Math.floor((timeDifference / (1000 * 60 * 60)) % 24);
      const minutes = Math.floor((timeDifference / (1000 * 60)) % 60);
      const seconds = Math.floor((timeDifference / 1000) % 60);
      return {
        days,
        hours,
        minutes,
        seconds,
      };
    })
      // Update the current time every second
    const updateTime = () => {
      currentTime.value = new Date();
    };

    onMounted(() => {
      // Start updating the current time
      setInterval(updateTime, 1000);
    });
</script>

<template>
<div class="time-count">
  <p>{{ remainingTime.days }}d |
    {{ remainingTime.hours }}h |
    {{ remainingTime.minutes }}m | 
    {{ remainingTime.seconds }}s
  </p>
</div>
</template>

<style lang="scss" scoped>

.time-count {
  p {
    font-size: 32px;
    font-weight: 700;
    font-family: 'Unbounded', sans-serif;
  }
}

</style>