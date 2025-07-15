<template>
  <div>
    <v-container class="fill-height">
      <v-row justify="center" align="center">
        <v-col cols="12" sm="8" md="6" lg="4">
          <v-card class="pa-6">
            <v-card-title class="text-center">
              AI 건설안전 전문가 챗봇
            </v-card-title>
            <v-card-text class="text-center">
              <p>플로팅 메뉴가 화면 우측에 표시됩니다.</p>
              <p>각 버튼을 클릭하여 기능을 테스트해보세요.</p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>

    <!-- 플로팅 메뉴 -->
    <FloatingMenu
      v-if="bShowFloatingMenu"
      @menu-click="handleMenuClick"
      @minimize="handleMinimize"
      @chatbot="handleChatbot"
    />

    <!-- 로봇 아이콘만 보이게 -->
    <v-btn
      v-else
      icon
      color="primary"
      class="floating-robot-btn"
      @click="handleChatbot"
      aria-label="챗봇 열기"
      size="large"
    >
      <v-icon>mdi-robot</v-icon>
    </v-btn>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import FloatingMenu from '~/components/common/FloatingMenu.vue'

// 플로팅 메뉴 표시 여부 상태
const bShowFloatingMenu = ref(true)

/**
 * <pre>
 * [메뉴 클릭 핸들러]
 * </pre>
 * 
 * @param action 클릭된 메뉴 액션
 */
const handleMenuClick = (action: string) => {
  console.log('메뉴 클릭:', action)
  // TODO: 각 메뉴 액션에 따른 처리 로직 구현
}

/**
 * <pre>
 * [최소화 핸들러]
 * </pre>
 */
const handleMinimize = () => {
  bShowFloatingMenu.value = false
}

/**
 * <pre>
 * [챗봇 핸들러]
 * </pre>
 */
const handleChatbot = () => {
  bShowFloatingMenu.value = true
}
</script>
<style>
.grid-stack-item-content {
  background-color: rgba(255, 255, 255, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
}
.floating-robot-btn {
  position: fixed;
  right: 32px;
  bottom: 32px;
  top: auto;
  transform: none;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #38404a;
  color: #fff;
  padding: 0;
  border: none;
  box-shadow: none;
  transition: background 0.18s, color 0.18s;
}
.floating-robot-btn:hover, .floating-robot-btn:focus {
  background: #495263;
  color: #fff;
}
.floating-robot-btn .v-icon {
  color: #fff;
  font-size: 2rem;
}
@media (max-width: 768px) {
  .floating-robot-btn {
    right: 10px;
    bottom: 10px;
  }
}
</style>
