<template>
  <div class="main-container">
    <div class="wrapper">
      <div class="output-container">
        <input
          readonly
          placeholder="Password"
          class="display"
          v-model="generatedPassword"
        />
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 448 512"
          @click="copyPassword"
        >
          <!--!Font Awesome Free 6.5.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.-->
          <path
            d="M384 336H192c-8.8 0-16-7.2-16-16V64c0-8.8 7.2-16 16-16l140.1 0L400 115.9V320c0 8.8-7.2 16-16 16zM192 384H384c35.3 0 64-28.7 64-64V115.9c0-12.7-5.1-24.9-14.1-33.9L366.1 14.1c-9-9-21.2-14.1-33.9-14.1H192c-35.3 0-64 28.7-64 64V320c0 35.3 28.7 64 64 64zM64 128c-35.3 0-64 28.7-64 64V448c0 35.3 28.7 64 64 64H256c35.3 0 64-28.7 64-64V416H272v32c0 8.8-7.2 16-16 16H64c-8.8 0-16-7.2-16-16V192c0-8.8 7.2-16 16-16H96V128H64z"
          />
        </svg>
      </div>
      <div class="input-container">
        <div class="password-length">
          <p>Password Length</p>
          <p class="step">{{ step }}</p>
        </div>
        <input
          type="range"
          class="slider"
          min="1"
          max="20"
          step="1"
          v-model="step"
          :style="{ backgroundSize: `${(step - 1) * 5.26}% 100%` }"
        />
        <div class="check">
          <input
            type="checkbox"
            id="Upper"
            v-model="upperCase"
            @click="upperCase = !upperCase"
          />
          <label for="Upper">Includes Uppercase letters</label>
        </div>
        <div class="check">
          <input
            type="checkbox"
            id="Lower"
            v-model="lowerCase"
            @click="lowerCase = !lowerCase"
          />
          <label for="Lower">Includes Lowercase letters</label>
        </div>
        <div class="check">
          <input
            type="checkbox"
            id="Number"
            v-model="number"
            @click="number = !number"
          />
          <label for="Number">Includes Numbers</label>
        </div>
        <div class="check">
          <input
            type="checkbox"
            id="Symbol"
            v-model="specialCharacter"
            @click="specialCharacter = !specialCharacter"
          />
          <label for="Symbol">Includes Symbols</label>
        </div>
        <div class="password-length">
          <p>Strength</p>
          <div class="strength" :style="[{'background-color':bgColor,'box-shadow':`${bgColor} 0px 0px 12px 1px`}]"></div>
        </div>
        <div class="btn">
          <button @click="generatePassword">GENERATE PASSWORD</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      step: 8,
      upperCase: true,
      lowerCase: true,
      number: true,
      specialCharacter: true,
      generatedPassword: "",
      bgColor: "gray",
    };
  },
  methods: {
    generatePassword() {
      const uppercaseChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
      const numberChars = "0123456789";
      const symbolChars = "!@#$%^&*()_+~`|}{[]\:;?><,./-=";

      let allowedChars = "";
      let password = "";
      if (
        this.upperCase ||
        this.lowerCase ||
        this.number ||
        this.specialCharacter
      ) {
        if (this.upperCase) allowedChars += uppercaseChars;
        if (this.lowerCase) allowedChars += lowercaseChars;
        if (this.number) allowedChars += numberChars;
        if (this.specialCharacter) allowedChars += symbolChars;

        for (let i = 0; i < this.step; i++) {
          const randomIndex = Math.floor(Math.random() * allowedChars.length);
          password += allowedChars[randomIndex];
        }

        this.generatedPassword = password;
        if (
          this.upperCase &&
          this.lowerCase &&
          (this.number || this.specialCharacter) &&
          this.step >= 8
        ) {
          this.bgColor = "#0f0";
        } else if (
          (this.lowerCase || this.upperCase) &&
          (this.number || this.specialCharacter) &&
          this.step >= 6
        ) {
          this.bgColor = "#ff0";
        } else {
          this.bgColor = "#f00";
        }
      }
    },
    copyPassword() {
      navigator.clipboard
        .writeText(this.generatedPassword)
        .then(() => {
          console.log("Password copied to clipboard");
        })
        .catch((error) => {
          console.error("Failed to copy password: ", error);
        });
    },
  },
};
</script>

<style scoped>
.main-container {
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  background: linear-gradient(
    90deg,
    rgba(238, 130, 238, 1) 0%,
    rgba(0, 0, 255, 1) 100%
  );
  gap: 20px;
}
.wrapper {
  width: 90%;
  max-width: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
}
.output-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0px 20px;
  background-color: black;
  height: 60px;
  border-radius: 10px;
  border-bottom: 0.35rem solid rgb(139, 2, 139);
}
svg {
  height: 23px;
  width: 23px;
  fill: white;
}
.input-container {
  width: 100%;
  padding: 10px;
  background-color: black;
  border-radius: 10px;
  color: white;
  padding: 1.5rem;
}

.input-container .password-length {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 10px;
}
.input-container .password-length p {
  color: white;
  font-size: 1.4rem;
  font-family: sans-serif;
}
.input-container .password-length .strength {
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 20px;
  background-color: rgb(255, 255, 0);
  box-shadow: rgb(255, 255, 0) 0px 0px 12px 1px;
}
.input-container .password-length .step {
  color: yellow;
}
.input-container .check label {
  font-size: 1.2rem;
  font-family: sans-serif;
  font-weight: 500;
}
.input-container .check input {
  appearance: none;
  width: 20px;
  height: 20px;
  border: 1px solid blueviolet;
  cursor: pointer;
  position: relative;
  border-radius: 0.35rem;
  color: white;
}
.input-container .check {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 10px 10px;
}
.input-container .check input:checked {
  background-color: blueviolet;
}
.input-container .check input:checked::before {
  content: "✔";
  position: absolute;
  color: var(--dk-text);
  font-size: 1.05rem;
  font-weight: 600;
  left: 50%;
  top: -2.5px;
  transform: translateX(-50%);
}
.slider {
  appearance: none;
  width: 100%;
  height: 0.75rem;
  cursor: pointer;
  background-color: violet;
  border-radius: 1rem;
  margin-top: 2rem;
  margin-bottom: 1.5rem;
  background-image: linear-gradient(rgb(165, 4, 165), rgb(184, 8, 184));
  background-repeat: no-repeat;
  border: none;
  outline: none;
  z-index: -1;
}
.btn {
  padding-top: 15px;
  text-align: center;
}
button {
  width: 85%;
  margin: auto;
  padding: 1rem 0;
  background-color: rgb(205, 18, 205);
  text-align: center;
  border-radius: 0.75rem;
  border-bottom: 0.35rem solid yellow;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: yellow;
  font-weight: bold;
  font-size: 1.25rem;
  cursor: pointer;
}
.display {
  width: 100%;
  background-color: transparent;
  padding: 1.15rem 1rem;
  padding-right: 1rem;
  color: yellow;
  font-weight: 600;
  font-size: 1.5rem;
  line-height: 30px;
  letter-spacing: 1px;
  padding-right: 3.25rem;
  border: none;
  outline: none;
}
</style>
