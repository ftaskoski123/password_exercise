<template>
  <div class="container">
    <div class="card">
      <h1>Please Enter Your Password</h1>
      <div class="input-container">
        <input
          :type="showPassword ? 'text' : 'password'"
          placeholder="Password"
          v-model="password"
        />
        <button class="toggle-password" @click="showPassword = !showPassword">
          <img
            v-if="showPassword"
            src="./svgs/eye_hide_password.svg"
            alt="Toggle Password Visibility"
          />
          <img
            v-else
            src="./svgs/eye_show_password.svg"
            alt="Toggle Password Visibility"
          />
        </button>
      </div>

      <button :disabled="!password" class="strength" @click="showscore = true">
        Check Strength
      </button>
    </div>

    <div v-if="showscore" class="score-container" @click="closescore">
      <div class="score">
        <h2>Your score is: {{ score }}</h2>
        <p>{{ feedback }}</p>
        <button @click="closescore">Close</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const password = ref<string>("");
const showPassword = ref<boolean>(false);
const showscore = ref<boolean>(false);

const closescore = () => {
  showscore.value = false;
  password.value = "";
};

const commonPasswords = ["password", "12345", "qwerty", "admin"];

const score = computed(() => {
  let sum = 0;

  if (password.value.length >= 12) {
    sum += 3;
  } else {
    sum += 2;
  }

  if (/[a-z]/.test(password.value)){

    sum += 1; 
  }

  if (/[A-Z]/.test(password.value)){

    sum += 1; 
  } 

  if (/[0-9]/.test(password.value)) {
    sum += 2;
  }

  if (/[^A-Za-z0-9]/.test(password.value)){

    sum += 2;
  } 

  if (!commonPasswords.includes(password.value.toLowerCase())) {

    sum += 1;
  }

  return sum;
});

const feedback = computed(() => {
  if (score.value <= 3){

    return "Weak password. Try adding more characters and using a mix of symbols.";
  }
  if (score.value > 3 && score.value <= 6){

    return "Moderate password. Consider adding more length, symbols, and uppercase characters.";
  }
  if (score.value > 6 && score.value < 9){

    return "Strong password. Almost there!";
  }
  return "Excellent password!";
});
</script>

<style scoped>
.container {
  align-items: center;
  justify-content: center;
  display: flex;
  height: 100vh;
  background-color: #f5f5f5;
}

.card {
  width: 600px;
  height: 300px;
  border-radius: 20px;
  background-color: #fff;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.input-container {
  display: flex;
  align-items: center;
  width: 80%;
  position: relative;
}

input[type="password"],
input[type="text"] {
  width: 100%;
  height: 45px;
  border: none;
  border-radius: 12px;
  padding: 12px 16px;
  font-size: 16px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
}

input::placeholder {
  color: #999;
  font-size: 14px;
}

input:focus {
  outline: none;
  background-color: #fff;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  border: 1px solid #4a90e2;
}

.toggle-password {
  background-color: transparent;
  border: none;
  cursor: pointer;
  position: absolute;
  right: 5px;
  top: 30%;
}

.toggle-password img {
  width: 24px;
  height: 24px;
}

.strength {
  margin-top: 20px;
  padding: 12px 24px;
  background: linear-gradient(45deg, #4a90e2, #56ccf2);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 20px;
  font-weight: bold;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  width: 60%;
  transition: all 0.3s ease;
}

.strength:hover {
  transform: translateY(-5px);
}

.score-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 100;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: opacity 0.3s ease;
}

.score {
  background-color: #fff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  width: 300px;
  text-align: center;
  animation: slideDown 0.8s ease-out;
}

.score h2 {
  margin-bottom: 20px;
}

.score button {
  padding: 10px 20px;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

.strength:disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

.score button:hover {
  background-color: #56ccf2;
}

@keyframes slideDown {
  from {
    transform: translateY(-100%);
  }
  to {
    transform: translateY(0);
  }
}
</style>
