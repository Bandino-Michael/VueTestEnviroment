<template>
    <div class="controls">
      <button class="btn btn-start" type="button" @click="startConnection()">Start</button>
      <button class="btn btn-stop" type="button">Stop</button>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    methods: {
        startConnection() {
            const socket = new WebSocket("ws://localhost:3002/agent");
    
            socket.addEventListener("open", () => {
            console.log("Connected to agent");
            });
    
            socket.addEventListener("message", ({ data }) => {
            const event = JSON.parse(data);
            console.log("Agent event:", event);
    
            if (event.type === "session.started") {
                // The bridge starts the agent session for you.
            }
            });
    
            socket.addEventListener("error", (event) => {
            console.error("WebSocket error:", event);
            });
    
            function sendEvent(event: { type: string; }) {
            if (socket.readyState !== WebSocket.OPEN) {
                console.warn("WebSocket is not open");
                return;
            }
            socket.send(JSON.stringify(event));
            }
    
            // End the conversation:
            function endConversation() {
            sendEvent({ type: "session.close" });
            }
        }
    }
})


</script>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  padding: 3rem 1rem;
}

.title {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
}

.title-image {
  width: 160px;
  height: 128px;
}

.controls {
  display: flex;
  gap: 1rem;
}

.btn {
  padding: 0.6rem 1.6rem;
  font-size: 1rem;
  font-weight: bold;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: #fff;
}

.btn-start {
  background-color: #2e7d32;
}

.btn-start:hover {
  background-color: #256428;
}

.btn-stop {
  background-color: #c62828;
}

.btn-stop:hover {
  background-color: #a62121;
}
</style>
