<template>
  <div id="app">
    <header>
      <h1>Song List</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Songs</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: ' Malo We ',
          artist: 'Kabza & Nobuhle',
          src: require('./assets/y2mate.com - Malo We.mp3')
        },
        {
          title: 'Matrimony',
          artist: 'Wale & Usher',
          src: require('./assets/y2mate.com - Wale Matrimony feat Usher Official Music Video.mp3')
        },
        {
          title: 'Im the one',
          artist: 'Dj Khaled',
          src: require('./assets/y2mate.com - DJ Khaled Im The One ft Justin Bieber Quavo Chance the Rapper Lil Wayne.mp3')
        },
        {
          title: 'Do you mind',
          artist: 'Dj Khaled',
          src: require('./assets/y2mate.com - Do You Mind Official Video.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: Monospace;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 30px;
	background-color: #1ebbb3;
	color: #FFF;
  font-weight: bold;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #bd393f;

  font-size: 30px;
  font-weight: bold;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: bold;
  

}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color:  #1ebbb3;
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color:  #1ebbb3;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #030303;
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 10px;
  font-size: 20px;
  font-weight: 700;
}
.playlist .song:hover {
  color: #FF5858;
}
.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right,  #1ebbb3, #FF5858);
}
</style>
