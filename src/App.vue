<script setup>
import { ref } from 'vue'

const username = ref('')
const userData = ref(null)
const message = ref('')

const searchUser = async () => {
  if (username.value === '') {
    message.value = '嘿！你忘記輸入名字了！'
    userData.value = null
    return
  }
  
  try {
    const response = await fetch(`https://api.github.com/users/${username.value}`)
    const data = await response.json()
    
    if (data.message === 'Not Found') {
      message.value = '找不到這個人！'
      userData.value = null
    } else {
      userData.value = data
      message.value = ''
    }
  } catch (error) {
    message.value = '網路出錯了！'
    userData.value = null
  }
}
</script>

<template>
  <div>
    <input v-model="username" placeholder="輸入GitHub用戶名" />
    <button @click="searchUser">搜尋用戶</button>
    
    <p v-if="message" style="color: red; margin-top: 10px;">{{ message }}</p>
    
    <div v-if="userData">
      <img :src="userData.avatar_url" alt="用戶頭像" style="width: 100px; height: 100px;" />
      <p>名字：{{ userData.name }}</p>
    </div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #f5f5f5;
}

div {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background: linear-gradient(135deg, #ffffff 0%, #f0f0f0 100%);
}

input {
  padding: 12px 16px;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 8px;
  margin-bottom: 12px;
  width: 100%;
  max-width: 300px;
  transition: border-color 0.3s;
}

input:focus {
  outline: none;
  border-color: #4a90e2;
}

button {
  padding: 12px 32px;
  font-size: 16px;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  margin-bottom: 20px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #357abd;
}

img {
  border-radius: 50%;
  margin-bottom: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

p {
  color: #333;
  font-size: 18px;
  font-weight: 500;
}
</style>