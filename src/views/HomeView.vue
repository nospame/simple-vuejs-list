<!-- 
Required: Fetch quotes from the source quotes.json and display the available information in a list-like structure (table/list)
  *Get data using axios
  *Set variable to data
 * V-for to parse data and display in a list or table
Required: Provide client-side pagination (up to 15 quotes per page)
  *Limit data to 15 rows at a time (discovery)
Required: Provide a way to filter between game and movie quotes
  *Add a selector input

Required: Provide a client-side search that filters by the quote text
  Add a search input (discovery or JS .includes? Something along those lines)

https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json

 -->

<script>import axios from "axios";

export default {
  data: function () {
    return {
      message: "Quotes",
      quotes: [],
      selectedQuotes: [],
      selectedTheme: '',
      startQuote: 0,
      endQuote: 14
    };
  },
  created: function () {
    this.getQuotes();
  },
  methods: {
    getQuotes: function () {
      axios.get('https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json')
        .then(response => {
          console.log(response.data);
          this.quotes = response.data;
          this.selectedQuotes = this.quotes;
        })

    },
    selectTheme: function () {
      this.selectedQuotes = this.quotes.filter(quote => quote.theme === this.selectedTheme)
    },
    nextPage: function () {
      this.startQuote = Math.min(this.startQuote + 15, this.selectedQuotes.length - 1)
      this.endQuote = this.startQuote + 15
    },
    prevPage: function () {
      this.startQuote = Math.max(this.startQuote - 15, 0)
      this.endQuote = this.startQuote + 15
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <select id="select-theme" name="select-theme" v-model="selectedTheme" v-on:change="selectTheme()">
      <option value="movies">Movies</option>
      <option value="games">Games</option>
    </select>
    <table>
      <tr>
        <th>Quote</th>
        <th>Context</th>
        <th>Source</th>
        <th>Theme</th>
      </tr>
      <tr v-for="quote in selectedQuotes.slice(startQuote, endQuote)">
        <td>{{ quote.quote }}</td>
        <td>{{ quote.context }}</td>
        <td>{{ quote.source }}</td>
        <td>{{ quote.theme }}</td>
      </tr>
      <button v-on:click="prevPage()">Previous Page</button>
      <button v-on:click="nextPage()">Next Page</button>
    </table>

  </div>
</template>

<style>
</style>