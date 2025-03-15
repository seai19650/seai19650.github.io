<template>
  <div class="section" id="chat-with-me">
    <h2 class="title is-2 has-text-centered">Ask Me Anything</h2>
    <div class="columns is-centered">
      <div class="column is-8">
        <div class="box">
          <div class="chat-history mb-4" ref="chatHistory">
            <div v-if="messages.length === 0" class="has-text-centered has-text-grey my-6">
              <p>Ask me anything about my skills, experience, or interests!</p>
            </div>
            <div v-for="(message, index) in messages" :key="index" class="message-container mb-3">
              <div :class="['message', message.isUser ? 'user-message' : 'ai-message']">
                <div class="message-header">
                  <p>{{ message.isUser ? 'You' : 'Dem\'s Assistant' }}</p>
                </div>
                <div class="message-body" v-if="isLoading && messages.length - 1 === index">
                    <p>Typing response ...</p>
                </div>
                <div v-else class="message-body" v-html="parseMessage(message.text)"></div>
              </div>
            </div>
          </div>
          <form @submit.prevent="sendMessage">
            <div class="field has-addons">
              <div class="control is-expanded">
                <input 
                  class="input is-medium" 
                  type="text" 
                  placeholder="Type your question here..." 
                  v-model="userInput"
                  :disabled="isLoading">
              </div>
              <div class="control">
                <button type="submit" class="button is-info is-medium" :disabled="isLoading || !userInput.trim()">
                  <span class="icon">
                    <i class="fas fa-paper-plane"></i>
                  </span>
                  <span>Send</span>
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChatWithMe",
  data() {
    return {
      userInput: "",
      messages: [],
      isLoading: false,
      ollamaEndpoint: process.env.VUE_APP_OLLAMA_ENDPOINT || "http://your-ollama-server-url",
      apiKey: process.env.VUE_APP_OLLAMA_API_KEY || "",
      streamingSpeed: 15, // milliseconds between character updates
    };
  },
  computed: {
    streamingMessage() {
      return this.messages.length > 0 && !this.messages[this.messages.length - 1].isUser ? 
        this.messages[this.messages.length - 1].text : '';
    }
  },
  methods: {
    parseMessage(message) {
      // Replace markdown syntax with HTML tags
      message = message.replace(/\*(.*?)\*/g, '<strong>$1</strong>');
      message = message.replace(/`(.*?)`/g, '<code>$1</code>');
      message = message.replace(/\[(.*?)\]\((.*?)\)/g, '<a href="$2">$1</a>');
      
      return message;
    },
    async sendMessage() {
      if (!this.userInput.trim()) return;
      
      // Add user message to chat
      this.messages.push({
        text: this.userInput,
        isUser: true
      });
      
      const query = this.userInput;
      this.userInput = "";
      this.isLoading = true;
      
      try {
        // Build headers with API key authentication
        const headers = {
          "Content-Type": "application/json",
        };
        
        // Add API key if available
        if (this.apiKey) {
          headers["Authorization"] = `Bearer ${this.apiKey}`;
        }
        
        // Add placeholder message for streaming response
        this.messages.push({
          text: "",
          isUser: false
        });
        
        // Use streaming mode
        const response = await fetch(`${this.ollamaEndpoint}/api/generate`, {
          method: "POST",
          headers: headers,
          body: JSON.stringify({
            model: "gemma2:9b", // Specify your Ollama model
            prompt: `User is asking about Kasidis Chaowvasin (Dem): ${query}`,
            stream: true,
          }),
        });
        
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        
        const reader = response.body.getReader();
        const decoder = new TextDecoder("utf-8");
        let buffer = "";
        
        let done = false;
        while (!done) {
          const result = await reader.read();
          done = result.done;
          if (done) break;
          
          const chunk = decoder.decode(result.value, { stream: true });
          buffer += chunk;
          
          // Process any complete JSON objects in the buffer
          let processedBuffer = this.processJSONBuffer(buffer);
          buffer = processedBuffer.remainder;
          
          for (const jsonObj of processedBuffer.objects) {
            if (jsonObj.response) {
              // Process one character at a time for visual effect
              await this.appendResponseText(jsonObj.response);
            }
          }
        }
        
        // Process any remaining data in the buffer
        if (buffer.trim()) {
          try {
            const jsonObj = JSON.parse(buffer);
            if (jsonObj.response) {
              await this.appendResponseText(jsonObj.response);
            }
          } catch (e) {
            console.error("Error parsing final buffer content:", e);
          }
        }
      } catch (error) {
        console.error("Error calling Ollama API:", error);
        this.messages.push({
          text: "Sorry, I encountered an error while processing your question. Please try again later.",
          isUser: false
        });
      } finally {
        this.isLoading = false;
        this.$nextTick(() => {
          this.scrollToBottom();
        });
      }
    },
    
    async appendResponseText(text) {
      // Add characters one by one with a slight delay to make the streaming visible
      for (let i = 0; i < text.length; i++) {
        await new Promise(resolve => setTimeout(resolve, this.streamingSpeed));
        this.messages[this.messages.length - 1].text += text[i];
        this.$nextTick(() => {
          this.scrollToBottom();
        });
      }
    },
    
    processJSONBuffer(buffer) {
      const result = {
        objects: [],
        remainder: buffer
      };
      
      // Find complete JSON objects in the buffer
      let startIndex = 0;
      let braceCount = 0;
      let inString = false;
      let escapeNext = false;
      
      for (let i = 0; i < buffer.length; i++) {
        const char = buffer[i];
        
        if (escapeNext) {
          escapeNext = false;
          continue;
        }
        
        if (char === '\\' && inString) {
          escapeNext = true;
          continue;
        }
        
        if (char === '"' && !escapeNext) {
          inString = !inString;
        }
        
        if (!inString) {
          if (char === '{') braceCount++;
          else if (char === '}') {
            braceCount--;
            
            // We found a complete JSON object
            if (braceCount === 0) {
              try {
                const jsonStr = buffer.substring(startIndex, i + 1);
                const jsonObj = JSON.parse(jsonStr);
                result.objects.push(jsonObj);
                startIndex = i + 1;
              } catch (e) {
                console.warn("Invalid JSON object found:", e);
              }
            }
          }
        }
      }
      
      // Set remainder to any incomplete JSON
      if (startIndex < buffer.length) {
        result.remainder = buffer.substring(startIndex);
      } else {
        result.remainder = "";
      }
      
      return result;
    },
    
    scrollToBottom() {
      if (this.$refs.chatHistory) {
        this.$refs.chatHistory.scrollTop = this.$refs.chatHistory.scrollHeight;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.chat-history {
  max-height: 400px;
  overflow-y: auto;
  padding: 1rem 0;
}

.message-container {
  display: flex;
  flex-direction: column;
}

.user-message {
  align-self: flex-end;
  
  .message-header {
    background-color: #209cee;
  }
}

.ai-message {
  align-self: flex-start;
  
  .message-header {
    background-color: #23d160;
  }
}

.message {
  max-width: 80%;
  border-radius: 6px;
  overflow: hidden;
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1);
  
  .message-header {
    padding: 0.5rem 1rem;
    color: white;
  }
  
  .message-body {
    padding: 1rem;
    background-color: #f5f5f5;
    border: none;
  }
}
</style>
