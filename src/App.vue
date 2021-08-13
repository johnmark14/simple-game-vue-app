<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>
  <div id="game">
    <section id="monster" class="container">
      <h2>Monster Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="monsterHealthBarStyle"></div>
      </div>
    </section>

    <section id="player" class="container">
      <h2>Your Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="playerHealthBarStyle"></div>
      </div>
    </section>

    <section id="controls">
      <button @click.prevent="attackMonster">ATTACK</button>
      <button
        @click.prevent="specialAttackMonster"
        :disabled="disabledSpecialAttackButton"
      >
        SPECIAL ATTACK
      </button>
      <button>HEAL</button>
      <button>SURRENDER</button>
    </section>

    <section id="log" class="container">
      <h2>Battle Log</h2>
      <ul></ul>
    </section>
  </div>
</template>

<script>
function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}
export default {
  name: "App",
  data() {
    return {
      myUseSpecialAttack: false,
      currentRound: 0,
      monsterHealth: 100,
      playerHealth: 100
    };
  },
  computed: {
    monsterHealthBarStyle() {
      return { width: this.monsterHealth + "%" };
    },
    playerHealthBarStyle() {
      return { width: this.playerHealth + "%" };
    },
    disabledSpecialAttackButton() {
      if (this.currentRound % 3 !== 0) {
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    attackMonster() {
      this.currentRound++;
      const attackValue = getRandomValue(5, 12);
      if (this.monsterHealth > 0) {
        this.monsterHealth -= attackValue;
      } else {
        this.monsterHealth = 0;
        this.currentRound = 3;
      }
      this.attackPlayer();
      console.log("attack Monster click! Health: " + this.monsterHealth);
    },
    attackPlayer() {
      const attackValue = getRandomValue(8, 15);
      if (this.monsterHealth > 0) {
        this.playerHealth -= attackValue;
      } else {
        this.playerHealth = 0;
        this.currentRound = 3;
      }
      console.log("attack Player click! Health: " + this.playerHealth);
    },
    specialAttackMonster() {
      this.currentRound++;
      const attackValue = getRandomValue(10, 25);
      if (this.monsterHealth > 0) {
        this.monsterHealth -= attackValue;
      } else {
        this.currentRound = 3;
      }
      this.attackPlayer();
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: "Jost", sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0.5rem;
  background-color: #880017;
  color: white;
  text-align: center;
  margin-bottom: 2rem;
}

section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}

.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}

#monster h2,
#player h2 {
  margin: 0.25rem;
}

#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #7700ff;
}

.log--monster {
  color: #da8d00;
}

.log--damage {
  color: red;
}

.log--heal {
  color: green;
}
</style>
