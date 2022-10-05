<template>
  <div class="container">
    <header>
      <h1>generátor mien</h1>
    </header>
    <main>
      <section>
        <h2>Vybrať pohlavie</h2>
        <button
          @click="options.gender = Gender.BOY"
          class="one"
          :class="options.gender === Gender.BOY && 'active-option'"
        >
          Chlapec
        </button>
        <button
          @click="options.gender = Gender.GIRL"
          class="two"
          :class="options.gender === Gender.GIRL && 'active-option'"
        >
          Dievča
        </button>
      </section>
      <section>
        <h2>Vybrať popularitu mien</h2>
        <button
          @click="options.popularity = Popularity.TRENDY"
          class="one"
          :class="options.popularity === Popularity.TRENDY && 'active-option'"
        >
          Trendové
        </button>
        <button
          @click="options.popularity = Popularity.UNIQUE"
          class="two"
          :class="options.popularity === Popularity.UNIQUE && 'active-option'"
        >
          Unikátne
        </button>
      </section>
      <section class="length">
        <h2>Vybať dľžku mien</h2>
        <button
          @click="options.length = Length.SHORT"
          :class="options.length === Length.SHORT && 'active-option'"
        >
          Krátke
        </button>
        <button
          @click="options.length = Length.ALL"
          class="middle"
          :class="options.length === Length.ALL && 'active-option'"
        >
          Všetky
        </button>
        <button
          @click="options.length = Length.LONG"
          :class="options.length === Length.LONG && 'active-option'"
        >
          Dlhé
        </button>
      </section>
      <aside>
        <button @click="extractData" class="select">Vyhľadať mená</button>
      </aside>
    </main>
    <footer>
      <ul>
        <li v-for="(personName, index) in filteredNames" :key="personName.id">
          {{ personName.name }}
          <span @click="deleteName(index)">X</span>
        </li>
      </ul>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length, names, Name } from "@/data";

interface optionsTypes {
  gender: Gender;
  popularity: Popularity;
  length: string;
}

const options = reactive<optionsTypes>({
  gender: Gender.GIRL,
  popularity: Popularity.TRENDY,
  length: Length.ALL,
});

const filteredNames = ref<Name[]>([]);

const extractData = () => {
  const data = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return name;
      else {
        return name.length === options.length;
      }
    });
  filteredNames.value = data.sort((a, b) => (a.name > b.name ? 1 : -1));
};

const deleteName = (index: number): void => {
  filteredNames.value.splice(index, 1);
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Kanit:wght@400;700&display=swap");
:root {
  --primary-color: #263849;
  --secondary-color: #41506b;
  --accent-color: #35bcbf;
}
.container {
  max-width: 50rem;
  margin: 0 auto;
  font-family: "Kanit", sans-serif;
  color: var(--primary-color);
}

h1 {
  text-align: center;
  text-transform: capitalize;
}

main {
  width: 100%;
  background-color: var(--accent-color);
  border-radius: 0.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

section {
  display: grid;
  place-items: center;
  grid-template-columns: repeat(6, 1fr);
}

section h2 {
  grid-column: span 6;
  margin: 1rem;
}

button {
  width: 100%;
  padding-block: 0.5rem;
  border: solid 0.0625rem var(--primary-color);
  transition: all 0.3s ease-in-out;
  cursor: pointer;
}

.length button:first-of-type {
  border-radius: 1rem 0 0 1rem;
  grid-column-start: 2;
}

.length :last-child {
  border-radius: 0 1rem 1rem 0;
  grid-column-start: 5;
}
.middle {
  grid-column: span 2;
}
.one {
  grid-column-start: 3;
  border-radius: 1rem 0 0 1rem;
}

.two {
  grid-column-start: 4;
  border-radius: 0 1rem 1rem 0;
}
.select {
  border-radius: 1rem;
  margin-block: 2rem;
}
.active-option {
  background-color: var(--secondary-color);
  color: antiquewhite;
}

ul {
  list-style: none;
  padding: 0.5rem;
  display: grid;
  gap: 0.5rem;
  grid-template-columns: repeat(3, 1fr);
}

li {
  position: relative;
  border-radius: 0.5rem;
  font-size: large;
  text-align: center;
  padding: 0.5rem 1rem;
  background-color: var(--accent-color);
}
li span {
  content: "X";
  position: absolute;
  top: -0.2rem;
  right: 0.25rem;
  padding: 0.25rem;
  cursor: pointer;
}
</style>
