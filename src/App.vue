<template>
  <div id="app">
    <Grid :rows="rows" v-on:update_grid="update_grid"/>
    <Clues v-bind:across="across" :down="down" v-on:highlight="change_highlight" />
  </div>
</template>

<script>
  import Grid from './components/Grid'
  import Clues from './components/Clues'

  export default {
    name: 'App',
    components: {
      Grid,
      Clues
    },
    data: function() {
      var size = 15;
      var rows = new Array;
      for(var i = 0; i < size; i++) {
        rows.push(new Array);
        for(var j = 0; j < size; j++) {
          rows[i].push({
            highlight: false,
            black: false,
            letter: "",
            number: ""
          });
        }
      }
      return {
        across: [],
        down: [],
        rows
      };
    },
    methods: {
      needs_number(r, c) {
        if(r == 0 || c == 0) {
          return true; // next to an edge
        } else {
          return (r > 0 && this.rows[r - 1][c].black) || (c > 0 && this.rows[r][c - 1].black);
        }
      },
      update_clues() {
        function check_down(rows, r, c) {
          while(r > 0) {
            if(rows[r][c].number) return false;
            if(rows[r][c].black) break; // ?
            r--;
          }
          return true;
        }
        function check_across(rows, r, c) {
          
          while(c >= 0) {
            if(rows[r][c].number) return false;
            if(rows[r][c].black) break; // ?
            c--;
          }
          return true;
        }

        var acrosses = [];
        var downs = [];
        for(var i = 0; i < this.rows.length; i++) {
          for(var j = 0; j < this.rows.length; j++) {
            if(this.rows[i][j].number) {
              // have to check all the way up, not just the square above
              if(i == 0) {
                downs.push(this.rows[i][j].number);
              } else if(i > 0 && check_down(this.rows, i - 1, j)) {
                downs.push(this.rows[i][j].number);
              }
              // ditto -- have to check all the way left.
              if(j == 0) {
                acrosses.push(this.rows[i][j].number);
              } else if(j > 0 && check_across(this.rows, i, j - 1)) {
                acrosses.push(this.rows[i][j].number);
              }
            }
          }
        }
        this.across = [];
        for(var an of acrosses) {
          this.across.push({
            number: an,
            clue: ""
          });
        }
        this.down = [];
        for(var dn of downs) {
          this.down.push({
            number: dn,
            clue: ""
          });
        }
        this.$forceUpdate();
      },
      update_grid() {
        var n = 1;
        for(var i = 0; i < this.rows.length; i++) {
          for(var j = 0; j < this.rows.length; j++) {
            if(this.rows[i][j].black) {
              this.$set(this.rows[i][j], 'number', '');
              this.$set(this.rows[i][j], 'letter', '');
            } else {
              if(this.needs_number(i, j)) {
                this.$set(this.rows[i][j], 'number', n++);
              } else {
                this.$set(this.rows[i][j], 'number', '');
              }
            }
          }
        }
        this.update_clues();
      },
      find_box(number) {
        for(var i = 0; i < this.rows.length; i++) {
          for(var j = 0; j < this.rows.length; j++) {
            if(this.rows[i][j].number == number) {
              return {row: i, column: j};
            }
          }
        }
      },
      change_highlight(direction, clue, to) {
        var box = this.find_box(clue);

        var next = box;
        while(next && next.row < this.rows.length && next.column < this.rows[0].length && !this.rows[next.row][next.column].black) {
          this.$set(this.rows[next.row][next.column], 'highlight', to);
          if(direction === "across") {
            next.column++;
          } else if(direction === "down") {
            next.row++;
          }
        }
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
  }
</style>
