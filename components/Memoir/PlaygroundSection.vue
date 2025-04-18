<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";
import { onClickOutside } from "@vueuse/core";
// import ChatBox from "~/components/ChatBox.vue";

const showChatModal = ref(false);
const showGameChoiceModal = ref(false);
const chatModalRef = ref(null);
const gameModalRef = ref(null);
const selectedGame = ref("");

// Close modal on outside click
onClickOutside(chatModalRef, () => {
  if (showChatModal.value) showChatModal.value = false;
});
onClickOutside(gameModalRef, () => {
  if (showGameChoiceModal.value) showGameChoiceModal.value = false;
});

// Optional: Close on ESC
const handleEscape = (e: KeyboardEvent) => {
  if (e.key === "Escape") {
    showChatModal.value = false;
    showGameChoiceModal.value = false;
  }
};

onMounted(() => window.addEventListener("keydown", handleEscape));
onBeforeUnmount(() => window.removeEventListener("keydown", handleEscape));

const openGameChoiceModal = () => {
  showGameChoiceModal.value = true;
};
const closeGameChoiceModal = () => {
  showGameChoiceModal.value = false;
};

const handleGameSelection = (game: string) => {
  selectedGame.value = game;
  closeGameChoiceModal();
  window.location.href = `/memoir/gaming-log/${game}`;
};
</script>

<template>
  <section id="playground" class="py-20 bg-[hsl(var(--alternate-background))]">
    <div class="max-w-4xl mx-auto px-6">
      <h2 class="text-3xl font-bold mb-6 text-foreground">🎠 Playground</h2>

      <!-- Playground Cards -->
      <div class="grid md:grid-cols-2 gap-6">
        <!-- ChatBox Playground -->
        <div
          v-motion-fade-visible
          class="bg-secondary border border-border rounded-2xl p-6 cursor-pointer transition-transform hover:scale-[1.02] duration-300"
          @click="showChatModal = true"
        >
          <h3 class="text-xl font-semibold text-primary mb-2">
            🧠 Talk with the Bot
          </h3>
          <p class="text-muted-foreground text-sm">
            Chat with Amiw’s thoughtful bot, always ready to listen 💬
          </p>
        </div>

        <!-- Gaming Log Card -->
        <div
          class="bg-secondary border border-border rounded-2xl p-6 cursor-pointer transition hover:scale-[1.02] duration-300 group"
          @click="openGameChoiceModal"
        >
          <h3 class="text-xl font-semibold text-primary mb-2">
            🎮 My Gaming Log
          </h3>
          <p class="text-muted-foreground text-sm">
            Track my progress in Mobile Legends & Valorant, from strategies to
            milestones!
          </p>
        </div>

        <!-- Other Cards -->
        <div
          v-motion-fade-visible
          class="bg-secondary border border-border rounded-2xl p-6 cursor-not-allowed transition hover:opacity-90 duration-300 group relative"
        >
          <h3 class="text-xl font-semibold text-primary mb-2">
            🌱 Sustainable Living
          </h3>
          <p class="text-muted-foreground text-sm">
            Learn to reduce waste, live minimally, and love the Earth in small
            everyday choices.
          </p>
          <div class="mt-4 text-center text-muted-foreground italic">
            Under Development
          </div>
        </div>

        <!-- Homesteading Projects -->
        <div
          v-motion-fade-visible
          class="bg-secondary border border-border rounded-2xl p-6 cursor-not-allowed transition hover:opacity-90 duration-300 group relative"
        >
          <h3 class="text-xl font-semibold text-primary mb-2">
            🏡 Homesteading Projects
          </h3>
          <p class="text-muted-foreground text-sm">
            Future space for garden logs, DIY tools, and seasonal harvest notes
            from a quiet life.
          </p>
          <div class="mt-4 text-center text-muted-foreground italic">
            Coming Soon...
          </div>
        </div>

        <!-- Pantry & Food Waste Tracker -->
        <div
          v-motion-fade-visible
          class="bg-secondary border border-border rounded-2xl p-6 cursor-not-allowed transition hover:opacity-90 duration-300 group relative"
        >
          <h3 class="text-xl font-semibold text-primary mb-2">
            🍽️ Food Waste Tracker
          </h3>
          <p class="text-muted-foreground text-sm">
            A simple tool to track what's in your pantry, save food, and plan
            meals more mindfully.
          </p>
          <div class="mt-4 text-center text-muted-foreground italic">
            Still in the kitchen...
          </div>
        </div>
      </div>
    </div>

    <!-- Game Selection Modal -->
    <transition name="fade">
      <div
        v-show="showGameChoiceModal"
        class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center px-4"
      >
        <div
          v-show="showGameChoiceModal"
          ref="gameModalRef"
          class="relative bg-white rounded-3xl shadow-2xl w-full max-w-xl p-8 border border-gray-200"
        >
          <button
            @click="closeGameChoiceModal"
            class="absolute top-4 right-6 text-gray-400 hover:text-red-500 transition text-xl"
          >
            ×
          </button>

          <h3 class="text-2xl font-semibold text-gray-800 mb-6 text-center">
            Choose a Game
          </h3>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <!-- Valorant Card -->
            <div
              class="cursor-pointer border border-gray-200 rounded-xl overflow-hidden hover:shadow-md transition"
              @click="handleGameSelection('valorant')"
            >
              <img
                src="/img/gaming-log/valorant.webp"
                alt="Valorant Cover"
                class="w-full h-32 object-cover"
              />
            </div>

            <!-- Mobile Legends Card -->
            <div
              class="cursor-pointer border border-gray-200 rounded-xl overflow-hidden hover:shadow-md transition"
              @click="handleGameSelection('mobile-legends')"
            >
              <img
                src="/img/gaming-log/ml.webp"
                alt="Mobile Legends Cover"
                class="w-full h-32 object-cover"
              />
            </div>

            <!-- Dota 2 Card -->
            <div
              class="cursor-pointer border border-gray-200 rounded-xl overflow-hidden hover:shadow-md transition"
              @click="handleGameSelection('mobile-legends')"
            >
              <img
                src="/img/gaming-log/dota2.webp"
                alt="Dota 2 Cover"
                class="w-full h-32 object-cover"
              />
            </div>
          </div>
        </div>
      </div>
    </transition>

    <!-- ChatBox Modal -->
    <transition name="fade">
      <div
        v-show="showChatModal"
        class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center px-4"
      >
        <div
          v-show="showChatModal"
          ref="chatModalRef"
          class="relative bg-secondary rounded-3xl shadow-2xl w-full max-w-2xl overflow-hidden border border-border"
        >
          <button
            @click="showChatModal = false"
            class="absolute top-4 right-6 text-muted-foreground hover:text-destructive transition text-xl"
          >
            ×
          </button>

          <div class="pt-8 pb-4 px-4 sm:px-8">
            <h3 class="text-xl font-semibold text-center text-primary mb-2">
              Amiw's Bot
            </h3>
            <p class="text-xs text-muted-foreground text-center">
              Powered by the <strong>Deepseek R1</strong> model, designed for
              engaging conversations and intelligent responses.
            </p>
            <ChatBox />

            <div class="mt-4 text-xs text-muted-foreground">
              <p class="italic">
                Disclaimer: The responses generated by this bot are based on a
                trained AI model and may not always be accurate. Please use
                discretion when interpreting or acting upon the information
                provided. And Amiw can not see your chat history, so feel free
                to interact! 🌸
              </p>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </section>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.scale-fade-enter-active,
.scale-fade-leave-active {
  transition: all 0.3s ease;
}
.scale-fade-enter-from,
.scale-fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>
