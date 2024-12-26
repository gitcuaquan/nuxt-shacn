<template>
  <div ref="ball" class="ball"></div>
</template>

<script setup>
  import { ref, onMounted, onUnmounted } from 'vue';

  // Tham chiếu đến hình cầu
  const ball = ref(null);

  // Kích thước hình cầu
  const ballSize = 500;

  // Vị trí chuột mục tiêu
  let targetX = 0;
  let targetY = 0;

  // Vị trí hiện tại của hình cầu
  let currentX = 0;
  let currentY = 0;

  // Tốc độ di chuyển
  const speed = 0.1;

  // Hàm xử lý sự kiện chuột di chuyển
  function handleMouseMove(event) {
    // Lấy vị trí chuột và điều chỉnh để tâm hình cầu trùng với chuột
    targetX = event.clientX - ballSize / 2; // Xác định vị trí X (trừ đi nửa kích thước)
    targetY = event.clientY - ballSize / 2; // Xác định vị trí Y (trừ đi nửa kích thước)
  }

  // Hàm cập nhật vị trí hình cầu
  function updatePosition() {
    // Di chuyển hình cầu dần dần đến vị trí mục tiêu
    currentX += (targetX - currentX) * speed;
    currentY += (targetY - currentY) * speed;

    if (ball.value) {
      ball.value.style.transform = `translate(${currentX}px, ${currentY}px)`;
    }

    requestAnimationFrame(updatePosition);
  }

  // Thiết lập sự kiện và animation
  onMounted(() => {
    window.addEventListener('mousemove', handleMouseMove);
    updatePosition();
  });

  // Dọn dẹp sự kiện khi component bị hủy
  onUnmounted(() => {
    window.removeEventListener('mousemove', handleMouseMove);
  });
</script>

<style scoped>
  .ball {
    position: fixed;
    /* Đảm bảo ball di chuyển trên toàn màn hình */
    width: 500px;
    /* Kích thước hình cầu */
    height: 500px;
    background: radial-gradient(circle, #7c3aeda8, transparent);
    filter: blur(20px);
    border-radius: 50%;
    opacity: 0.4;
    pointer-events: none;
    /* Để chuột không bị cản bởi hình cầu */
    z-index: -1;
    /* Hiển thị trên các phần tử khác */
  }
</style>
