# Voice Alert System - Complete Implementation

## Overview

A comprehensive multi-language voice alert system for fishermen entering restricted zones. Supports English, Tamil, and Hindi with proper TTS language codes.

## Features

- ✅ **Multi-language Support**: English (en-IN), Tamil (ta-IN), Hindi (hi-IN)
- ✅ **Web Speech API**: Browser-based TTS with language detection
- ✅ **Offline Fallback**: Pre-recorded audio files for offline support
- ✅ **React Integration**: Custom hooks for easy component integration
- ✅ **Audio Generation**: Tools to create audio files for offline use
- ✅ **Persistent Settings**: Language preferences saved to localStorage

## Quick Start

### 1. Basic Usage

```typescript
import { useVoiceAlerts } from './hooks/useVoiceAlerts';

const MyComponent = () => {
  const { playAlert, stopAlert } = useVoiceAlerts({
    language: 'tamil',
    enableVoice: true,
  });

  const handleZoneViolation = () => {
    playAlert('entering_restricted');
  };

  return (
    <button onClick={handleZoneViolation}>
      Test Tamil Alert
    </button>
  );
};
```

### 2. Language Switching

```typescript
import { useLanguage } from './contexts/LanguageContext';
import { useVoiceAlerts } from './hooks/useVoiceAlerts';

const AlertComponent = () => {
  const { language, setLanguage } = useLanguage();
  const { playAlert } = useVoiceAlerts({ language });

  return (
    <div>
      <select onChange={(e) => setLanguage(e.target.value as Language)}>
        <option value="english">English</option>
        <option value="tamil">தமிழ்</option>
        <option value="hindi">हिंदी</option>
      </select>
      
      <button onClick={() => playAlert('danger_alert')}>
        Play Alert
      </button>
    </div>
  );
};
```

### 3. Advanced Configuration

```typescript
import { VoiceAlertManager } from './utils/voiceAlerts';

const manager = new VoiceAlertManager({
  language: 'hindi',
  enableVoice: true,
  volume: 0.8,
  rate: 0.9,
  pitch: 1.1,
});

// Play alert with custom settings
await manager.playAlert('warning_alert', {
  volume: 1.0,
  rate: 1.0,
});

// Update configuration
manager.setLanguage('tamil');
manager.setEnabled(false);
```

## Available Messages

```typescript
const messages = {
  entering_restricted: {
    english: "You are entering a restricted zone!",
    tamil: "நீங்கள் தடைசெய்யப்பட்ட பகுதியில் நுழைகிறீர்கள்!",
    hindi: "आप प्रतिबंधित क्षेत्र में प्रवेश कर रहे हैं!"
  },
  approaching_restricted: {
    english: "You are approaching a restricted zone!",
    tamil: "நீங்கள் தடைசெய்யப்பட்ட பகுதியை நெருங்குகிறீர்கள்!",
    hindi: "आप प्रतिबंधित क्षेत्र के करीब पहुंच रहे हैं!"
  },
  danger_alert: {
    english: "Danger Alert!",
    tamil: "அபாய எச்சரிக்கை!",
    hindi: "खतरे की चेतावनी!"
  },
  warning_alert: {
    english: "Warning!",
    tamil: "எச்சரிக்கை!",
    hindi: "चेतावनी!"
  }
};
```

## Language Codes

```typescript
const LANGUAGE_CODES = {
  english: 'en-IN',  // Indian English
  tamil: 'ta-IN',    // Tamil (India)
  hindi: 'hi-IN',    // Hindi (India)
};
```

## Audio File Generation

### Generate All Audio Files

```typescript
import { audioGenerator } from './utils/audioGenerator';

// Generate audio files for all messages and languages
await audioGenerator.generateAllAudioFiles();
```

### Generate for Specific Language

```typescript
// Generate only Tamil audio files
await audioGenerator.generateLanguageAudioFiles('tamil');
```

### Generate for Specific Message

```typescript
// Generate danger_alert in all languages
await audioGenerator.generateMessageAudioFiles('danger_alert');
```

## File Structure

```
src/
├── utils/
│   ├── voiceAlerts.ts          # Core TTS system
│   └── audioGenerator.ts       # Audio file generation
├── hooks/
│   └── useVoiceAlerts.ts       # React integration
├── contexts/
│   └── LanguageContext.tsx     # Language state management
└── components/
    └── AlertBox.tsx            # Updated alert component

public/
└── audio/
    ├── english/
    │   ├── danger_alert.mp3
    │   ├── warning_alert.mp3
    │   └── entering_restricted.mp3
    ├── tamil/
    │   ├── danger_alert.mp3
    │   ├── warning_alert.mp3
    │   └── entering_restricted.mp3
    └── hindi/
        ├── danger_alert.mp3
        ├── warning_alert.mp3
        └── entering_restricted.mp3
```

## Testing

### Test Voice Alerts

```typescript
// Test all languages
const testLanguages = async () => {
  const { playAlert } = useVoiceAlerts();
  
  // Test English
  await playAlert('danger_alert', { language: 'english' });
  
  // Test Tamil
  await playAlert('danger_alert', { language: 'tamil' });
  
  // Test Hindi
  await playAlert('danger_alert', { language: 'hindi' });
};
```

### Browser Console Test

```javascript
// Test in browser console
const utterance = new SpeechSynthesisUtterance('அபாய எச்சரிக்கை!');
utterance.lang = 'ta-IN';
window.speechSynthesis.speak(utterance);
```

## Mobile Support

### React Native Integration

```typescript
// For React Native, use react-native-tts
import Tts from 'react-native-tts';

const playMobileAlert = async (text: string, language: string) => {
  await Tts.setDefaultLanguage(language);
  await Tts.speak(text);
};
```

### Cordova/PhoneGap

```typescript
// Using cordova-plugin-tts
const playCordovaAlert = (text: string, language: string) => {
  TTS.speak({
    text: text,
    locale: language,
    rate: 0.9,
    pitch: 1.0,
  });
};
```

## Offline Support

### Preload Audio Files

```typescript
import { audioFileUtils } from './utils/audioGenerator';

// Preload critical alerts
await audioFileUtils.preloadAudioFiles(
  ['danger_alert', 'warning_alert'],
  ['english', 'tamil', 'hindi']
);
```

### Check File Availability

```typescript
const isAudioAvailable = await audioFileUtils.fileExists(
  '/audio/tamil/danger_alert.mp3'
);
```

## Error Handling

```typescript
const { playAlert } = useVoiceAlerts();

try {
  await playAlert('danger_alert');
} catch (error) {
  console.error('Voice alert failed:', error);
  // Fallback to visual alert or notification
}
```

## Performance Tips

1. **Preload Audio**: Load critical audio files on app start
2. **Cache Voices**: Cache available voices for faster switching
3. **Debounce Alerts**: Prevent rapid-fire alerts
4. **Volume Control**: Allow users to adjust volume
5. **Offline Mode**: Provide offline audio files

## Browser Compatibility

- ✅ Chrome/Edge: Full Web Speech API support
- ✅ Firefox: Limited support
- ✅ Safari: Basic support
- ✅ Mobile browsers: Varies by platform

## Troubleshooting

### No Voice Output
1. Check browser console for errors
2. Verify language codes are correct
3. Test with simple English text first
4. Check if Web Speech API is supported

### Wrong Language
1. Verify LANGUAGE_CODES mapping
2. Check browser voice availability
3. Test with different browsers
4. Use offline audio files as fallback

### Performance Issues
1. Reduce audio file sizes
2. Use lower quality settings
3. Implement audio caching
4. Debounce rapid alerts

## Example Integration

```typescript
// Complete example
import React from 'react';
import { useLanguage } from './contexts/LanguageContext';
import { useVoiceAlerts } from './hooks/useVoiceAlerts';

const FishingZoneAlert = () => {
  const { language, voiceAlertsEnabled } = useLanguage();
  const { playAlert, stopAlert } = useVoiceAlerts({
    language,
    enableVoice: voiceAlertsEnabled,
  });

  const handleZoneEntry = () => {
    playAlert('entering_restricted');
  };

  const handleZoneApproach = () => {
    playAlert('approaching_restricted');
  };

  const handleDanger = () => {
    playAlert('danger_alert');
  };

  return (
    <div>
      <button onClick={handleZoneEntry}>Zone Entry Alert</button>
      <button onClick={handleZoneApproach}>Zone Approach Alert</button>
      <button onClick={handleDanger}>Danger Alert</button>
      <button onClick={stopAlert}>Stop All Alerts</button>
    </div>
  );
};

export default FishingZoneAlert;
```

This system provides complete voice alert functionality with multi-language support, offline capabilities, and easy React integration.
