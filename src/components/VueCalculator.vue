<template>
  <h1>Calculator</h1>
  <div class="calculator">
    <div class="display">{{ display }}</div>

    <div class="buttons">
      <button @click="clearDisplay" class="clear-all-button">AC</button>
      <button @click="clearLastDigit" class="clear-button">C</button>
      
      <button @click="appendToDisplay('%')" class="operator-button">%</button>
      <button @click="appendToDisplay('/')" class="operator-button">/</button>
      
      <button @click="appendToDisplay('7')" class="number-button">7</button>
      <button @click="appendToDisplay('8')" class="number-button">8</button>
      <button @click="appendToDisplay('9')" class="number-button">9</button>
      <button @click="appendToDisplay('*')" class="operator-button">*</button>

      <button @click="appendToDisplay('4')" class="number-button">4</button>
      <button @click="appendToDisplay('5')" class="number-button">5</button>
      <button @click="appendToDisplay('6')" class="number-button">6</button>
      <button @click="appendToDisplay('-')" class="operator-button">-</button>
  

      <button @click="appendToDisplay('1')" class="number-button">1</button>
      <button @click="appendToDisplay('2')" class="number-button">2</button>
      <button @click="appendToDisplay('3')" class="number-button">3</button>

      
      <button @click="appendToDisplay('+')" class="operator-button">+</button>
      <button @click="appendToDisplay('.')" class="number-button">.</button>
      <button @click="appendToDisplay('0')" class="number-button">0</button>
     
      <button @click="calculateResult" class="equal-button">=</button>
    </div>
  </div>
</template>


<script>
import { ref, onMounted, onBeforeUnmount } from 'vue';

export default {
  setup() {
    const display = ref('0');

    const appendToDisplay = (value) => {
      if (display.value === '0') {
        display.value = value;
      } else {
        display.value += value;
      }
    };

    const clearDisplay = () => {
      display.value = '0';
    };

    const clearLastDigit = () => {
      display.value = display.value.slice(0, -1);
      if (display.value === '') {
        display.value = '0';
      }
    };

    const calculateResult = () => {
      try {
        const expression = display.value.replace(/%/g, '/ 100');
        const result = eval(expression);
        display.value = result.toString();
      } catch (error) {
        display.value = 'Error';
      }
    };

    const handleKeyboardInput = (event) => {
      const key = event.key;
      if (/^[0-9+\-*./%]$/.test(key)) {
        if (display.value === '0') {
          display.value = key;
        } else {
          appendToDisplay(key);
        }
      } else if (key === 'Enter') {
        calculateResult();
      } else if (key === 'Backspace') {
        clearLastDigit();
      }
    };

    onMounted(() => {
      window.addEventListener('keydown', handleKeyboardInput);
    });

    onBeforeUnmount(() => {
      window.removeEventListener('keydown', handleKeyboardInput);
    });

    return {
      display,
      appendToDisplay,
      clearDisplay,
      clearLastDigit,
      calculateResult,
    };
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
}

.calculator {
  border: 15px solid silver;
  width: 300px;
  margin: 0 auto;
  text-align: center;
  background-color: black;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.display {
  font-size: 32px;
  margin-bottom: 10px;
  background-color: silver;
  color: black;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 1);
  text-shadow: 1px 1px 1px rgba(0, 0, 0, 1);
  overflow: auto;
  padding: 20px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

.number-button,
.operator-button,
.clear-button,
.equal-button,
.clear-all-button {
  font-size: 18px;
  padding: 15px;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.number-button {
  background-color: #3498db;
  color: #fff;
}

.operator-button {
  background-color: #8B4513;
  color: #fff;
}

.clear-button {
  background-color: #f39c12;
  color: #fff;
}

.equal-button {
  font-size: 24px;
  background-color: #2ecc71;
  color: #fff;
  grid-column: 3 / span 2; 
  grid-row: 5; 
}

.clear-all-button {
  background-color: #e74c3c;
  color: #fff;
}

.number-button:hover {
  background-color: #2980b9;
}

.operator-button:hover {
  background-color: #A0522D;
}

.clear-button:hover {
  background-color: #d4ac0d; 
}

.equal-button:hover {
  background-color: #27ae60; 
}

.clear-all-button:hover {
  background-color: #c0392b;
}

</style>






  
  