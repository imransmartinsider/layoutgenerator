<template>
  <div class="container">
    <h2>Dynamic HTML and CSS Template Generator</h2>
    <form @submit.prevent="addRow" class="form">
      <div v-for="(columns, index) in rows" :key="index" class="form-group">
        <label>Number of columns in row {{ index + 1 }}:</label>
        <input type="number" v-model="rows[index]" min="1" required class="input" />
      </div>
      <div class="buttons">
        <button type="button" @click="addNewRow" class="button">Add Another Row</button>
        <button type="submit" class="button primary">Generate Template</button>
      </div>
    </form>

    <div v-if="generatedHTML" class="result">
      <h3>Generated HTML:</h3>
      <pre class="code">{{ generatedHTML }}</pre>
      <h3>Generated CSS:</h3>
      <pre class="code">{{ generatedCSS }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rows: [1],
      generatedHTML: '',
      generatedCSS: '',
    };
  },
  methods: {
    addNewRow() {
      this.rows.push(1);
    },
    addRow() {
      const htmlParts = [];
      const cssParts = [];
      cssParts.push('<style>');
      this.rows.forEach((columns, rowIndex) => {
        const rowHTML = [];
        rowHTML.push(`<div class="row row-${rowIndex + 1}">`);
        for (let colIndex = 0; colIndex < columns; colIndex++) {
          rowHTML.push(`<div class="column col-${rowIndex + 1}-${colIndex + 1}">Fake text</div>`);
        }
        rowHTML.push('</div>');
        htmlParts.push(rowHTML.join('\n'));

        cssParts.push(`.row-${rowIndex + 1} { display: flex; }`);
        for (let colIndex = 0; colIndex < columns; colIndex++) {
          cssParts.push(`.col-${rowIndex + 1}-${colIndex + 1} { flex: 1; padding: 10px; border: 1px solid #ccc; }`);
        }
      });
      cssParts.push('</style>');

      this.generatedHTML = htmlParts.join('\n');
      this.generatedCSS = cssParts.join('\n');
    },
  },
};
</script>

<style>
/* Import CSS file directly */
@import './styles.css';
/* Additional styles here if needed */
</style>
