<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Music Player</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: 
                linear-gradient(135deg, rgba(102, 126, 234, 0.9) 0%, rgba(118, 75, 162, 0.9) 100%),
                url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320"><path fill="%23ff6b6b" fill-opacity="0.1" d="M0,96L48,112C96,128,192,160,288,176C384,192,480,192,576,165.3C672,139,768,85,864,80C960,75,1056,117,1152,144C1248,171,1344,181,1392,186.7L1440,192L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"></path></svg>'),
                url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320"><path fill="%23feca57" fill-opacity="0.05" d="M0,224L48,213.3C96,203,192,181,288,181.3C384,181,480,203,576,213.3C672,224,768,224,864,208C960,192,1056,160,1152,144C1248,128,1344,128,1392,128L1440,128L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"></path></svg>'),
                radial-gradient(circle at 20% 80%, rgba(120, 119, 198, 0.3) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(255, 107, 107, 0.3) 0%, transparent 50%),
                radial-gradient(circle at 40% 40%, rgba(254, 202, 87, 0.2) 0%, transparent 50%);
            background-size: cover, 100% 100%, 100% 100%, 800px 800px, 600px 600px, 400px 400px;
            background-position: center, bottom, top, left, right, center;
            background-attachment: fixed;
            min-height: 100vh;
            color: #ffffff;
            overflow-x: hidden;
        }

        .container {
            max-width: 400px;
            margin: 0 auto;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 28px;
            font-weight: 300;
            margin-bottom: 10px;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .player-container {
            background: url('hh.jpg');
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        .album-art {
            width: 200px;
            height: 200px;
            border-radius: 15px;
            margin: 0 auto 20px;
            background: linear-gradient(45deg, #ff9a9e, #fecfef);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 60px;
            color: rgba(255, 255, 255, 0.7);
            position: relative;
            overflow: hidden;
            animation: rotate 20s linear infinite;
        }

        .album-art.playing {
            animation-play-state: running;
        }

        .album-art.paused {
            animation-play-state: paused;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .album-art img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 15px;
        }

        .song-info {
            text-align: center;
            margin-bottom: 25px;
        }

        .song-title {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 5px;
            color: #ffffff;
        }

        .song-artist {
            font-size: 16px;
            color: rgba(255, 255, 255, 0.7);
        }

        .progress-container {
            margin-bottom: 20px;
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 3px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, #ff6b6b, #feca57);
            border-radius: 3px;
            width: 0%;
            transition: width 0.1s;
        }

        .time-info {
            display: flex;
            justify-content: space-between;
            margin-top: 8px;
            font-size: 12px;
            color: rgba(255, 255, 255, 0.7);
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin-bottom: 25px;
        }

        .control-btn {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #ffffff;
            cursor: pointer;
            transition: all 0.3s;
            backdrop-filter: blur(10px);
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.1);
        }

        .play-btn {
            width: 60px;
            height: 60px;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
        }

        .play-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
        }

        .volume-slider {
            flex: 1;
            height: 4px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 2px;
            outline: none;
            -webkit-appearance: none;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            border-radius: 50%;
            cursor: pointer;
        }

        .file-input-container {
            position: relative;
            margin-bottom: 20px;
        }

        .file-input {
            position: absolute;
            opacity: 0;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }

        .file-input-label {
            display: block;
            padding: 15px;
            background: rgba(255, 255, 255, 0.1);
            border: 2px dashed rgba(255, 255, 255, 0.3);
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .file-input-label:hover {
            background: rgba(255, 255, 255, 0.2);
            border-color: rgba(255, 255, 255, 0.5);
        }

        .playlist-toggle {
            width: 100%;
            padding: 12px;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            border-radius: 10px;
            color: #ffffff;
            cursor: pointer;
            margin-bottom: 10px;
            transition: all 0.3s;
            backdrop-filter: blur(10px);
        }

        .playlist-toggle:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .playlist-container {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            max-height: 0;
            overflow: hidden;
            transition: all 0.3s ease;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .playlist-container.show {
            max-height: 300px;
            padding: 15px;
        }

        .playlist {
            max-height: 250px;
            overflow-y: auto;
        }

        .playlist-item {
            display: flex;
            align-items: center;
            padding: 10px;
            margin-bottom: 5px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .playlist-item:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .playlist-item.active {
            background: linear-gradient(45deg, rgba(255, 107, 107, 0.3), rgba(254, 202, 87, 0.3));
        }

        .playlist-item-info {
            flex: 1;
        }

        .playlist-item-title {
            font-size: 14px;
            font-weight: 500;
            margin-bottom: 2px;
        }

        .playlist-item-duration {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.6);
        }

        .remove-btn {
            background: rgba(255, 107, 107, 0.2);
            border: none;
            border-radius: 4px;
            color: #ff6b6b;
            cursor: pointer;
            padding: 4px 8px;
            font-size: 12px;
            transition: all 0.3s;
        }

        .remove-btn:hover {
            background: rgba(255, 107, 107, 0.3);
        }

        .playlist::-webkit-scrollbar {
            width: 6px;
        }

        .playlist::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
        }

        .playlist::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 3px;
        }

        .equalizer {
            display: flex;
            justify-content: center;
            align-items: end;
            gap: 3px;
            height: 20px;
            margin-left: 10px;
        }

        .equalizer.playing .bar {
            animation: bounce 1s infinite;
        }

        .bar {
            width: 3px;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            border-radius: 2px;
        }

        .bar:nth-child(1) { height: 8px; animation-delay: 0.1s; }
        .bar:nth-child(2) { height: 12px; animation-delay: 0.2s; }
        .bar:nth-child(3) { height: 16px; animation-delay: 0.3s; }
        .bar:nth-child(4) { height: 10px; animation-delay: 0.4s; }
        .bar:nth-child(5) { height: 14px; animation-delay: 0.5s; }

        @keyframes bounce {
            0%, 100% { transform: scaleY(1); }
            50% { transform: scaleY(0.3); }
        }

        .drop-zone {
            border: 3px dashed rgba(255, 255, 255, 0.5);
            background: rgba(255, 255, 255, 0.1);
        }

        @media (max-width: 480px) {
            .container {
                padding: 15px;
            }
            
            .album-art {
                width: 180px;
                height: 180px;
            }
            
            .controls {
                gap: 15px;
            }
            
            .control-btn {
                width: 45px;
                height: 45px;
            }
            
            .play-btn {
                width: 55px;
                height: 55px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎵 RAKAT MUSIK PLAYER</h1>
        </div>

        <div class="player-container">
            <div class="album-art paused" id="albumArt">
                🎵
            </div>

            <div class="song-info">
                <div class="song-title" id="songTitle">No song selected</div>
                <div class="song-artist" id="songArtist">Choose a song to play</div>
            </div>

            <div class="progress-container">
                <div class="progress-bar" id="progressBar">
                    <div class="progress" id="progress"></div>
                </div>
                <div class="time-info">
                    <span id="currentTime">0:00</span>
                    <span id="duration">0:00</span>
                </div>
            </div>

            <div class="controls">
                <button class="control-btn" id="prevBtn">⏮️</button>
                <button class="control-btn play-btn" id="playBtn">▶️</button>
                <button class="control-btn" id="nextBtn">⏭️</button>
                <button class="control-btn" id="shuffleBtn">🔀</button>
                <button class="control-btn" id="repeatBtn">🔁</button>
            </div>

            <div class="volume-container">
                <span>🔊</span>
                <input type="range" class="volume-slider" id="volumeSlider" min="0" max="100" value="50">
                <span id="volumeValue">50%</span>
            </div>
        </div>

        <div class="file-input-container">
            <input type="file" class="file-input" id="fileInput" accept="audio/*" multiple>
            <label for="fileInput" class="file-input-label" id="fileInputLabel">
                🎵 Drag & Drop or Click to Add Songs
            </label>
        </div>

        <button class="playlist-toggle" id="playlistToggle">
            📋 Show Playlist (<span id="songCount">0</span> songs)
        </button>

        <div class="playlist-container" id="playlistContainer">
            <div class="playlist" id="playlist"></div>
        </div>

        <audio id="audioPlayer"></audio>
    </div>

    <script>
        class MusicPlayer {
            constructor() {
                this.audioPlayer = document.getElementById('audioPlayer');
                this.playBtn = document.getElementById('playBtn');
                this.prevBtn = document.getElementById('prevBtn');
                this.nextBtn = document.getElementById('nextBtn');
                this.shuffleBtn = document.getElementById('shuffleBtn');
                this.repeatBtn = document.getElementById('repeatBtn');
                this.progressBar = document.getElementById('progressBar');
                this.progress = document.getElementById('progress');
                this.currentTime = document.getElementById('currentTime');
                this.duration = document.getElementById('duration');
                this.volumeSlider = document.getElementById('volumeSlider');
                this.volumeValue = document.getElementById('volumeValue');
                this.fileInput = document.getElementById('fileInput');
                this.fileInputLabel = document.getElementById('fileInputLabel');
                this.playlist = document.getElementById('playlist');
                this.playlistContainer = document.getElementById('playlistContainer');
                this.playlistToggle = document.getElementById('playlistToggle');
                this.songCount = document.getElementById('songCount');
                this.songTitle = document.getElementById('songTitle');
                this.songArtist = document.getElementById('songArtist');
                this.albumArt = document.getElementById('albumArt');

                this.songs = [];
                this.currentSongIndex = 0;
                this.isPlaying = false;
                this.isShuffling = false;
                this.isRepeating = false;
                this.playlistVisible = false;

                this.initializeEventListeners();
                this.setupDragAndDrop();
            }

            initializeEventListeners() {
                this.playBtn.addEventListener('click', () => this.togglePlay());
                this.prevBtn.addEventListener('click', () => this.previousSong());
                this.nextBtn.addEventListener('click', () => this.nextSong());
                this.shuffleBtn.addEventListener('click', () => this.toggleShuffle());
                this.repeatBtn.addEventListener('click', () => this.toggleRepeat());
                
                this.progressBar.addEventListener('click', (e) => this.setProgress(e));
                this.volumeSlider.addEventListener('input', (e) => this.setVolume(e.target.value));
                this.fileInput.addEventListener('change', (e) => this.handleFileSelect(e));
                this.playlistToggle.addEventListener('click', () => this.togglePlaylist());

                this.audioPlayer.addEventListener('timeupdate', () => this.updateProgress());
                this.audioPlayer.addEventListener('loadedmetadata', () => this.updateDuration());
                this.audioPlayer.addEventListener('ended', () => this.handleSongEnd());
                this.audioPlayer.addEventListener('loadstart', () => this.handleLoadStart());
                this.audioPlayer.addEventListener('canplay', () => this.handleCanPlay());
            }

            setupDragAndDrop() {
                ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                    this.fileInputLabel.addEventListener(eventName, this.preventDefaults, false);
                });

                ['dragenter', 'dragover'].forEach(eventName => {
                    this.fileInputLabel.addEventListener(eventName, () => {
                        this.fileInputLabel.classList.add('drop-zone');
                    }, false);
                });

                ['dragleave', 'drop'].forEach(eventName => {
                    this.fileInputLabel.addEventListener(eventName, () => {
                        this.fileInputLabel.classList.remove('drop-zone');
                    }, false);
                });

                this.fileInputLabel.addEventListener('drop', (e) => {
                    const files = e.dataTransfer.files;
                    this.handleFiles(files);
                }, false);
            }

            preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }

            handleFileSelect(event) {
                this.handleFiles(event.target.files);
            }

            handleFiles(files) {
                Array.from(files).forEach(file => {
                    if (file.type.startsWith('audio/')) {
                        const url = URL.createObjectURL(file);
                        const song = {
                            name: file.name.replace(/\.[^/.]+$/, ""),
                            url: url,
                            file: file,
                            duration: 0
                        };
                        this.songs.push(song);
                        this.loadSongDuration(song, this.songs.length - 1);
                    }
                });
                this.updatePlaylist();
                this.updateSongCount();
                
                if (this.songs.length === 1) {
                    this.loadSong(0);
                }
            }

            loadSongDuration(song, index) {
                const tempAudio = new Audio();
                tempAudio.addEventListener('loadedmetadata', () => {
                    song.duration = tempAudio.duration;
                    this.updatePlaylistItem(index);
                });
                tempAudio.src = song.url;
            }

            updatePlaylistItem(index) {
                const playlistItem = this.playlist.children[index];
                if (playlistItem) {
                    const durationElement = playlistItem.querySelector('.playlist-item-duration');
                    if (durationElement) {
                        durationElement.textContent = this.formatTime(this.songs[index].duration);
                    }
                }
            }

            togglePlay() {
                if (this.songs.length === 0) return;
                
                if (this.isPlaying) {
                    this.pause();
                } else {
                    this.play();
                }
            }

            play() {
                if (this.songs.length === 0) return;
                
                this.audioPlayer.play().then(() => {
                    this.isPlaying = true;
                    this.playBtn.textContent = '⏸️';
                    this.albumArt.classList.remove('paused');
                    this.albumArt.classList.add('playing');
                    this.updateActivePlaylistItem();
                }).catch(error => {
                    console.error('Error playing audio:', error);
                });
            }

            pause() {
                this.audioPlayer.pause();
                this.isPlaying = false;
                this.playBtn.textContent = '▶️';
                this.albumArt.classList.remove('playing');
                this.albumArt.classList.add('paused');
            }

            previousSong() {
                if (this.songs.length === 0) return;
                
                this.currentSongIndex = this.currentSongIndex > 0 ? this.currentSongIndex - 1 : this.songs.length - 1;
                this.loadSong(this.currentSongIndex);
                if (this.isPlaying) {
                    this.play();
                }
            }

            nextSong() {
                if (this.songs.length === 0) return;
                
                if (this.isShuffling) {
                    this.currentSongIndex = Math.floor(Math.random() * this.songs.length);
                } else {
                    this.currentSongIndex = this.currentSongIndex < this.songs.length - 1 ? this.currentSongIndex + 1 : 0;
                }
                this.loadSong(this.currentSongIndex);
                if (this.isPlaying) {
                    this.play();
                }
            }

            toggleShuffle() {
                this.isShuffling = !this.isShuffling;
                this.shuffleBtn.style.color = this.isShuffling ? '#feca57' : '#ffffff';
            }

            toggleRepeat() {
                this.isRepeating = !this.isRepeating;
                this.repeatBtn.style.color = this.isRepeating ? '#feca57' : '#ffffff';
            }

            togglePlaylist() {
                this.playlistVisible = !this.playlistVisible;
                if (this.playlistVisible) {
                    this.playlistContainer.classList.add('show');
                    this.playlistToggle.textContent = `📋 Hide Playlist (${this.songs.length} songs)`;
                } else {
                    this.playlistContainer.classList.remove('show');
                    this.playlistToggle.textContent = `📋 Show Playlist (${this.songs.length} songs)`;
                }
            }

            loadSong(index) {
                if (index >= 0 && index < this.songs.length) {
                    this.currentSongIndex = index;
                    const song = this.songs[index];
                    this.audioPlayer.src = song.url;
                    this.songTitle.textContent = song.name;
                    this.songArtist.textContent = 'Unknown Artist';
                    this.updateActivePlaylistItem();
                }
            }

            setProgress(event) {
                const clickX = event.offsetX;
                const width = this.progressBar.offsetWidth;
                const duration = this.audioPlayer.duration;
                
                if (duration) {
                    this.audioPlayer.currentTime = (clickX / width) * duration;
                }
            }

            setVolume(value) {
                this.audioPlayer.volume = value / 100;
                this.volumeValue.textContent = value + '%';
            }

            updateProgress() {
                const duration = this.audioPlayer.duration;
                const currentTime = this.audioPlayer.currentTime;
                
                if (duration) {
                    const progressPercent = (currentTime / duration) * 100;
                    this.progress.style.width = progressPercent + '%';
                    this.currentTime.textContent = this.formatTime(currentTime);
                }
            }

            updateDuration() {
                const duration = this.audioPlayer.duration;
                if (duration) {
                    this.duration.textContent = this.formatTime(duration);
                }
            }

            handleSongEnd() {
                if (this.isRepeating) {
                    this.audioPlayer.currentTime = 0;
                    this.play();
                } else {
                    this.nextSong();
                }
            }

            handleLoadStart() {
                this.songTitle.textContent = 'Loading...';
            }

            handleCanPlay() {
                if (this.songs.length > 0) {
                    this.songTitle.textContent = this.songs[this.currentSongIndex].name;
                }
            }

            updatePlaylist() {
                this.playlist.innerHTML = '';
                this.songs.forEach((song, index) => {
                    const item = document.createElement('div');
                    item.className = 'playlist-item';
                    item.innerHTML = `
                        <div class="playlist-item-info">
                            <div class="playlist-item-title">${song.name}</div>
                            <div class="playlist-item-duration">${this.formatTime(song.duration)}</div>
                        </div>
                        <div class="equalizer" style="display: none;">
                            <div class="bar"></div>
                            <div class="bar"></div>
                            <div class="bar"></div>
                            <div class="bar"></div>
                            <div class="bar"></div>
                        </div>
                        <button class="remove-btn" onclick="player.removeSong(${index})">✕</button>
                    `;
                    
                    item.addEventListener('click', (e) => {
                        if (!e.target.classList.contains('remove-btn')) {
                            this.loadSong(index);
                            this.play();
                        }
                    });
                    
                    this.playlist.appendChild(item);
                });
            }

            updateActivePlaylistItem() {
                const items = this.playlist.querySelectorAll('.playlist-item');
                const equalizers = this.playlist.querySelectorAll('.equalizer');
                
                items.forEach((item, index) => {
                    item.classList.remove('active');
                    equalizers[index].style.display = 'none';
                    equalizers[index].classList.remove('playing');
                });
                
                if (items[this.currentSongIndex]) {
                    items[this.currentSongIndex].classList.add('active');
                    equalizers[this.currentSongIndex].style.display = 'flex';
                    if (this.isPlaying) {
                        equalizers[this.currentSongIndex].classList.add('playing');
                    }
                }
            }

            updateSongCount() {
                this.songCount.textContent = this.songs.length;
                this.playlistToggle.textContent = `📋 ${this.playlistVisible ? 'Hide' : 'Show'} Playlist (${this.songs.length} songs)`;
            }

            removeSong(index) {
                if (index === this.currentSongIndex && this.isPlaying) {
                    this.pause();
                }
                
                URL.revokeObjectURL(this.songs[index].url);
                this.songs.splice(index, 1);
                
                if (index < this.currentSongIndex) {
                    this.currentSongIndex--;
                } else if (index === this.currentSongIndex) {
                    if (this.currentSongIndex >= this.songs.length) {
                        this.currentSongIndex = 0;
                    }
                    if (this.songs.length > 0) {
                        this.loadSong(this.currentSongIndex);
                    } else {
                        this.audioPlayer.src = '';
                        this.songTitle.textContent = 'No song selected';
                        this.songArtist.textContent = 'Choose a song to play';
                        this.currentTime.textContent = '0:00';
                        this.duration.textContent = '0:00';
                        this.progress.style.width = '0%';
                    }
                }
                
                this.updatePlaylist();
                this.updateSongCount();
            }

            formatTime(seconds) {
                if (isNaN(seconds)) return '0:00';
                
                const minutes = Math.floor(seconds / 60);
                const remainingSeconds = Math.floor(seconds % 60);
                return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`;
            }
        }

        const player = new MusicPlayer();
    </script>
</body>
</html>
