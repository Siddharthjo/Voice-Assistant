# Offline Voice Assistant

A production-ready Progressive Web App (PWA) that provides offline-first voice interaction capabilities with local speech-to-text, OpenAI integration, and local text-to-speech synthesis.

## 🚀 Features

### Core Functionality
- **Offline-First Architecture**: Works completely offline after initial load (except OpenAI calls)
- **Local Speech-to-Text**: Whisper WASM integration for privacy-focused transcription
- **AI-Powered Responses**: OpenAI GPT integration for intelligent conversations
- **Local Text-to-Speech**: Cached TTS models for immediate audio playback
- **Real-time Performance Monitoring**: Sub-1200ms response time targeting

### Technical Features
- **Progressive Web App**: Installable with full offline capabilities
- **Service Worker Caching**: Aggressive caching of models and static assets
- **Web Workers**: Dedicated workers for STT and TTS processing
- **Real-time Audio Processing**: Live transcription with audio level visualization
- **Responsive Design**: Optimized for mobile and desktop usage

## 🛠 Tech Stack

- **Framework**: Next.js 13+ with TypeScript
- **UI**: Tailwind CSS + shadcn/ui components
- **Audio Processing**: Web Audio API + MediaRecorder API
- **STT**: Whisper.cpp WASM (via Web Worker)
- **TTS**: Coqui TTS (via Web Worker)
- **AI**: OpenAI GPT-3.5-turbo
- **PWA**: Service Worker + Web App Manifest
- **Icons**: Lucide React

## 📋 Prerequisites

- Node.js 18+ and npm
- OpenAI API key
- Modern browser with Web Audio API support
- HTTPS connection (required for microphone access)

## 🚀 Quick Start

### 1. Clone and Install

```bash
git clone <repository-url>
cd offline-voice-assistant
npm install
```

### 2. Environment Setup

Create a `.env.local` file:

```bash
cp .env.example .env.local
```

Add your OpenAI API key:

```env
OPENAI_API_KEY=your_openai_api_key_here
NODE_ENV=development
```

### 3. Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### 4. Production Build

```bash
npm run build
npm start
```

## 🏗 Architecture

### Application Flow

```
User Speech → Microphone → Audio Chunks → Whisper Worker → Transcript
                                                              ↓
Audio Playback ← TTS Worker ← OpenAI Response ← API Route ← Final Transcript
```

### Key Components

#### Service Worker (`/public/sw.js`)
- Caches static assets and model files
- Enables offline functionality
- Handles background sync for queued requests

#### Web Workers
- **Whisper Worker** (`/public/workers/whisper-worker.js`): Handles STT processing
- **TTS Worker** (`/public/workers/tts-worker.js`): Manages text-to-speech synthesis

#### Core Utilities
- **AudioProcessor** (`/lib/audio-utils.ts`): Audio recording and playback management
- **ServiceWorkerManager** (`/lib/service-worker-utils.ts`): PWA functionality
- **PerformanceMonitor**: Real-time latency tracking

## 🎯 Performance Targets

| Metric | Target | Description |
|--------|--------|-------------|
| STT Latency | <300ms | Speech-to-text processing time |
| API Latency | <800ms | OpenAI response time |
| TTS Latency | <200ms | Text-to-speech synthesis |
| **Total Response** | **<1200ms** | End-to-end interaction time |

## 🔧 Configuration

### Audio Settings

The app uses optimized audio constraints for best quality:

```javascript
{
  audio: {
    sampleRate: 16000,
    channelCount: 1,
    echoCancellation: true,
    noiseSuppression: true,
    autoGainControl: true
  }
}
```

### OpenAI Configuration

Default settings for voice interactions:

```javascript
{
  model: 'gpt-3.5-turbo',
  max_tokens: 150,
  temperature: 0.7
}
```

## 📱 PWA Installation

### Desktop
1. Visit the app in Chrome/Edge
2. Click the install icon in the address bar
3. Follow the installation prompts

### Mobile
1. Open the app in mobile browser
2. Tap "Add to Home Screen" from browser menu
3. App will install as native-like experience

## 🔒 Privacy & Security

- **Local Processing**: STT and TTS happen entirely on-device
- **No Audio Storage**: Audio data is processed in memory only
- **Secure API**: OpenAI calls use server-side API routes
- **HTTPS Required**: Ensures secure microphone access

## 🐛 Troubleshooting

### Common Issues

#### Microphone Access Denied
- Ensure HTTPS connection
- Check browser permissions
- Try refreshing and allowing microphone access

#### Service Worker Not Registering
```javascript
// Check browser console for errors
// Ensure running on HTTPS or localhost
// Clear browser cache and reload
```

#### Models Not Loading
- Check network connection for initial load
- Verify service worker registration
- Clear cache and reload application

#### Poor Audio Quality
- Check microphone permissions
- Ensure quiet environment
- Verify audio constraints in browser settings

### Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| Web Audio API | ✅ | ✅ | ✅ | ✅ |
| MediaRecorder | ✅ | ✅ | ✅ | ✅ |
| Service Worker | ✅ | ✅ | ✅ | ✅ |
| Web Workers | ✅ | ✅ | ✅ | ✅ |
| PWA Install | ✅ | ✅ | ✅ | ✅ |

## 🚀 Deployment

### Vercel (Recommended)

```bash
npm install -g vercel
vercel
```

### Netlify

```bash
npm run build
# Upload 'out' folder to Netlify
```

### Docker

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

## 🔄 Development Workflow

### Adding New Features

1. **Audio Processing**: Extend `AudioProcessor` class
2. **Worker Communication**: Update worker message handlers
3. **UI Components**: Use shadcn/ui components
4. **Performance**: Add metrics to `PerformanceMonitor`

### Testing

```bash
# Run development server
npm run dev

# Build and test production
npm run build
npm start

# Test PWA functionality
# Use Chrome DevTools > Application > Service Workers
```

## 📊 Monitoring

### Performance Metrics

The app tracks and displays:
- STT processing latency
- OpenAI API response time
- TTS synthesis duration
- Total interaction latency
- Audio level visualization
- Network status

### Debug Information

Enable debug logging:

```javascript
// In browser console
localStorage.setItem('debug', 'voice-assistant:*');
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Use TypeScript for all new code
- Follow existing code style and patterns
- Add performance monitoring for new features
- Test offline functionality thoroughly
- Update documentation for API changes

## 🙏 Acknowledgments

- [Whisper.cpp](https://github.com/ggerganov/whisper.cpp) for WASM STT
- [Coqui TTS](https://github.com/coqui-ai/TTS) for local synthesis
- [OpenAI](https://openai.com) for GPT integration
- [shadcn/ui](https://ui.shadcn.com) for beautiful components
