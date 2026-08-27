# 🎵 Aesthetic Music Player

A beautiful, offline-first web music player with PWA support, real-time audio visualization, and optional Google Drive sync. Built with vanilla JavaScript, no frameworks required.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-active-success.svg)

## ✨ Features

- **🎨 Aesthetic UI** - Glassmorphism design with smooth animations
- **📱 PWA Support** - Install as a native app on mobile/desktop
- **💾 Offline Playback** - Service Worker caching for seamless offline experience
- **🎛️ Audio Visualization** - Real-time frequency bars using Web Audio API
- **🌈 Mood Ambiance** - Dynamic background gradients (Chill, Energetic, Focus)
- **☁️ Google Drive Integration** - Stream and cache audio files from Drive
- **🗂️ Local Playlist** - IndexedDB-powered persistent storage
- **⌨️ Keyboard Accessible** - Full keyboard navigation support

## 🚀 Quick Start

### Installation

1. Clone the repository:
```bash
git clone https://github.com/nuthatchy/aesthetic-music-player.git
cd aesthetic-music-player
```

2. Serve locally (required for Service Workers):
```bash
# Using Python 3
python -m http.server 8000

# OR using Node.js http-server
npx http-server

# OR using Live Server (VS Code extension)
```

3. Open `http://localhost:8000` in your browser

## 📋 File Structure

```
aesthetic-music-player/
├── index.html          # Main UI, database logic, visualizer
├── sw.js              # Service Worker for offline caching
├── manifest.json      # PWA configuration
├── .gitignore         # Git ignore patterns
├── LICENSE            # MIT License
└── README.md          # This file
```

## 🎯 Usage

### Local Music
1. Click **"➕ Upload Local Audio"** and select audio files from your device
2. Files are automatically saved to IndexedDB for offline access
3. Click any track in the Library to play

### Google Drive Sync (Optional)

#### Setup Instructions

1. **Create a Google Cloud Project**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project (name it "Aesthetic Music Player" or similar)
   - Enable the **Google Drive API**:
     - Navigate to "APIs & Services" > "Library"
     - Search for "Google Drive API"
     - Click "Enable"

2. **Create OAuth 2.0 Credentials**
   - Go to "APIs & Services" > "Credentials"
   - Click "Create Credentials" > "OAuth 2.0 Client IDs"
   - Choose **Web application**
   - Add authorized JavaScript origins:
     - `http://localhost:8000` (for local development)
     - Your production domain (e.g., `https://yourdomain.com`)
   - Add authorized redirect URIs:
     - `http://localhost:8000/` (for local development)
     - Your production URL
   - Copy the **Client ID** (you'll need this next)

3. **Configure Consent Screen** (required for production)
   - Go to "APIs & Services" > "OAuth consent screen"
   - Set to "External" app
   - Add required fields:
     - App name: "Aesthetic Music Player"
     - User support email: your email
     - Developer contact: your email
   - Add test users or publish the app

4. **Add Your Client ID to the App**
   - Open `index.html`
   - Find line with `const CLIENT_ID = 'YOUR_CLIENT_ID_HERE.apps.googleusercontent.com';`
   - Replace with your actual Client ID from step 2
   - Save the file

5. **Test the Integration**
   - Refresh your browser
   - Click "☁️ Sync Google Drive"
   - Grant permission to access your Drive
   - Audio files will load automatically and cache locally

### Mood Ambiance
Click any mood button to change the background gradient:
- **Chill** - Calm blue tones
- **Energetic** - Vibrant red/orange
- **Focus** - Deep ocean blues

## 🔧 Configuration

### Customize Colors
Edit the CSS variables in `index.html` (lines 8-11):

```css
:root {
  --bg-gradient: linear-gradient(135deg, #1e1e24, #2a2a36);
  --accent-color: #bb86fc;
  --text-color: #ffffff;
}
```

### Add More Moods
Edit the `moods` object in `index.html` (lines ~240):

```javascript
const moods = {
  chill: 'linear-gradient(135deg, #2b5876, #4e4376)',
  energetic: 'linear-gradient(135deg, #ff416c, #ff4b2b)',
  focus: 'linear-gradient(135deg, #0f2027, #203a43, #2c5364)',
  // Add your own:
  sunset: 'linear-gradient(135deg, #ff6b6b, #feca57)'
};
```

Then add a button in the mood selector:
```html
<button onclick="setMood('sunset')">Sunset</button>
```

## 🌐 Deployment

### Deploy to GitHub Pages

1. Push your code to GitHub:
```bash
git add .
git commit -m "Deploy to GitHub Pages"
git push origin main
```

2. Enable GitHub Pages:
   - Go to repository Settings > Pages
   - Set source to "Deploy from a branch"
   - Select "main" branch
   - Save

3. Your app will be live at: `https://yourusername.github.io/aesthetic-music-player/`

4. **Important**: Update Google Drive OAuth credentials:
   - Add your GitHub Pages URL to authorized JavaScript origins
   - Add your GitHub Pages URL to authorized redirect URIs
   - Update `CLIENT_ID` in `index.html` with the same ID (it works for both localhost and production)

### Deploy to Vercel / Netlify

1. **Vercel**:
```bash
npm i -g vercel
vercel
```

2. **Netlify**:
   - Connect your GitHub repo
   - No build command needed
   - Deploy from root directory

## ⚙️ Technical Details

### Storage
- **IndexedDB (Dexie)**: Persists songs locally for offline playback
- **LocalStorage**: Could be used for mood preferences (future enhancement)
- **Service Worker Cache**: Caches app assets for offline-first experience

### APIs Used
- **Web Audio API**: Real-time frequency visualization
- **Google Drive API**: Stream audio files from cloud
- **Service Workers**: Offline caching strategy
- **IndexedDB**: Local database via Dexie

### Dependencies
- **[Dexie.js](https://dexie.org/)** - IndexedDB wrapper (CDN)
- **[Google Identity Services SDK](https://developers.google.com/identity)** - OAuth 2.0 (CDN)

## 🐛 Troubleshooting

### "CORS error" when syncing Google Drive
- Ensure your domain is added to authorized JavaScript origins in Google Cloud Console
- Service workers must run on HTTPS (or localhost for development)

### Audio files not playing
- Check browser DevTools Console for errors
- Ensure audio format is supported (MP3, WAV, OGG, M4A)
- Clear IndexedDB if corrupted: `await db.songs.clear()`

### PWA not installing
- App must be served over HTTPS (or localhost)
- `manifest.json` must be valid JSON
- Service Worker must be registered successfully

### Service Worker not updating
- Hard refresh: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac)
- Uninstall PWA and reinstall

## 🔐 Privacy & Security

- **Local Playback**: All offline music stays on your device
- **Google Drive**: Only fetches audio files you authorize via OAuth
- **No Tracking**: No analytics, no user tracking
- **No Backend**: Everything runs client-side (except Drive API)

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🙏 Acknowledgments

- Glassmorphism design inspiration
- Web Audio API documentation
- Dexie.js community for excellent IndexedDB abstraction

## 📞 Support

If you find a bug or have a suggestion:
1. Check existing [Issues](https://github.com/nuthatchy/aesthetic-music-player/issues)
2. Create a new Issue with details and screenshots
3. Include browser/OS information

---

**Enjoy your music! 🎵**
