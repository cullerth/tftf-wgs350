<template>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
  </head>
  <div>
    <h4>for WGS 350:</h4>
    <h4><i>Feminist Futures</i></h4> 
    <p>
      The Thing From the Future is an imagination game meant to spur creative and critical thinking.<br>
      To play the game, you draw a set of cards that prompt you to create an object from an alternative future.
    </p>
  </div>

  <div class="wrapper">
    <div class="box a">
      <p> ARC </p>
      <h4>{{ arccards[0] }}</h4>
    </div>
    <div class="box b">
      <p> OBJECT </p>
      <h4> there is a/an <br><em>{{ objectcards[0] }}</em></h4>
    </div>
    <div class="box c">
      <p> TERRAIN </p>
      <h4> related to <br><em>{{ terraincards[0] }}</em></h4>
    </div>
    <div class="box c">
      <p> FEMINIST ARC </p>
      <h4> and influenced by <br><em>{{ femmanifestos[0] }}</em>.</h4>
    </div>
    

    
  </div>
  
  <div class="app">
    <br>
    <p><button v-on:click="deal_cards()" class="btn btn-primary">Deal Cards</button></p>
    <br>
    <h2><i> What is it? </i></h2>
    <p>
      Use <a href="https://docs.google.com/presentation/d/1z85wILoPLngr1Zz6oJzAUfolEx2P_PFhNK81GBjdwLs/copy" target="_blank">this template</a> to respond to the prompt. 
      <br>
    </p>  
    <h3>  </h3>
    <h3> About the card decks: </h3>
      <ul class="list-group">
        <li v-for="(deck, index) in card_decks" :key="index">{{deck.desc}}</li>
        <br>
        <li><i> Drawn a wildcard? Fill in your own idea! </i></li>
      </ul>
      <br>
  </div>

  <div>
    <p>
      Made by <a href="https://www.lib.ncsu.edu/spaces/innovation-studio">the Innovation Studio at NC State University Libraries</a><br>
      Based on <a href="http://situationlab.org/project/the-thing-from-the-future/">TFTF by Situation Lab</a><br>
      2021 <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/">CC-BY-NC-SA</a>
    </p>
  </div>
</template>

<script>
export default {
  name: 'tftf',
  props: {
    msg: String
  },
  inject:['papaparse'],
  data() {
    return {
        arccards: [],
        objectcards: [],
        terraincards: [],
        femmanifestos: [],
        card_decks: [ 
          {deck: "Arc", desc: "ARC outlines the type of future world that the thing comes from, and how far away it is from today."}, // There are four types of Arc, each an umbrella for countless possible scenarios: growth, collapse, discipline, transformation.
          {deck: "Object" , desc: "OBJECT is the focus for your imagination: a specific cultural artifact that reveals something about how this future is different from today."},
          {deck: "Terrain", desc: "TERRAIN is the thematic context or location where this object could be found in that future."},
          {deck: "Feminist Manifesto", desc: "FEMINIST ARC refers to the feminist theory that you can use to build and interpret this particular future and your thing's place in it."}
      ] 
    }
  },
  created() {
    this.parse()
  },
  methods: {
    parse() {
      var vue = this;
        this.papaparse.parse('https://raw.githubusercontent.com/cullerth/tftf-wgs350/main/src/data/tftf_data_all.csv', {
        header: true,
        download: true, 
        complete: function(results) {
          console.log('parsing done', results)
          vue.cards = results['data']

          var arccards = []// vue.cards[0]['Description']
          var objectcards = []
          var terraincards = []
          var femmanifestos = []
          // var arccards = []
          vue.cards.forEach(function(card) {
            if (card['Deck'] == 'Arc') {
              arccards.push(card['Description'])
            } else if (card['Deck'] == 'Object') {
              objectcards.push(card['Title'])
            } else if (card['Deck'] == 'Terrain') {
              terraincards.push(card['Title'])
            } else if (card['Deck'] == 'Feminist Manifesto') {
              femmanifestos.push(card['Title'])
            }
            //else if (card['Deck'] == 'Arc') {
            //   arccards.push('In a "' + card['Title'] + '" future,' + card['Description'] + ", ")
            // } 
          });

          vue.arccards = arccards;
          vue.objectcards = objectcards;
          vue.terraincards = terraincards;
          vue.femmanifestos = femmanifestos;

        }
      });
    },
    deal_cards() {
      this.arccards[0] = this.arccards[Math.floor(Math.random() * this.arccards.length)]
      this.objectcards[0] = this.objectcards[Math.floor(Math.random() * this.objectcards.length)]
      this.terraincards[0] = this.terraincards[Math.floor(Math.random() * this.terraincards.length)]
      this.femmanifestos[0] = this.femmanifestos[Math.floor(Math.random() * this.femmanifestos.length)]
    }
  }
}
</script>

<style scoped>
h3 {
    margin: 40px 0 0;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: inline-block;
    margin: 0 10px;
}

@media screen and (min-width: 601px){
  .wrapper {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));;
    background-color: #fff;
    color: rgb(202, 189, 189);
    max-width: 1000px;
    /* max-height: 800px; */
    height: 410px; 
    margin: 0 auto; 
    padding: 25px; 
    column-gap: 15px;
    row-gap: 15px; 
  }
  .box {
    background-color: #fff;
    color: black;
    border-style: solid;
    border-color: black;
    border-radius: 23px;
    border-width: 10px;
    padding: 25px;
    font-size: 150%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    text-align: center;
    }
  .wrapper .box:nth-child(1) {    background:#8ec741;           }
  .wrapper .box:nth-child(2) {    background:#00bef3;            }
  .wrapper .box:nth-child(3) {    background:#f36d7d;         }
  .wrapper .box:nth-child(4) {
  background:#bc8cbeff}
}

@media screen and (max-width: 600px){
  .wrapper {
    display: grid;
    grid-template-rows: repeat(4, minmax(0, 1fr));
    background-color: #fff;
    color: rgb(202, 189, 189);
    max-width: 300px;
    /* max-height: 800px; */
    height: 1400px;
    margin: 0 auto; 
    padding: 25px; 
    column-gap: 15px;
    row-gap: 15px; 
  }
  .box {
    background-color: #fff;
    color: black;
    border-style: solid;
    border-color: black;
    border-radius: 23px;
    border-width: 10px;
    padding: 25px;
    font-size: 150%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    text-align: center;
  }
  .wrapper .box:nth-child(1) {    background:#8ec741;           }
  .wrapper .box:nth-child(2) {    background:#00bef3;            }
  .wrapper .box:nth-child(3) {    background:#f36d7d;         }
  .wrapper .box:nth-child(4) {
  background:#bc8cbeff}
}
</style>