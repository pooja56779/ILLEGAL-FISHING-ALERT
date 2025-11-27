# 🚨 Voice Alert Troubleshooting Guide

## Quick Fix Steps

### Step 1: Check Browser Console
1. **Open Developer Tools**: Press `F12` or right-click → "Inspect"
2. **Go to Console tab**
3. **Look for errors** related to speech synthesis
4. **Check for messages** starting with 🔊, 🎤, ❌

### Step 2: Test Voice Debugger
1. **Go to Dashboard page**
2. **Scroll down** to see "Voice Debug Tool"
3. **Click "Check Browser Support"** first
4. **Click "Test English Speech"** - this should work
5. **Click "Test Tamil Speech"** - this might not work
6. **Click "Test Hindi Speech"** - this might not work

### Step 3: Common Issues & Solutions

#### Issue 1: No Speech Synthesis Support
**Symptoms**: Console shows "Speech synthesis not supported"
**Solution**: 
- Use Chrome or Edge browser
- Update your browser
- Try incognito mode

#### Issue 2: No Voices Available
**Symptoms**: Console shows "Available Voices: 0"
**Solution**:
- Wait 2-3 seconds and try again
- Refresh the page
- Check internet connection

#### Issue 3: Speech Starts But No Sound
**Symptoms**: Console shows "Speech started" but no audio
**Solution**:
- Check system volume
- Check browser volume (click speaker icon in tab)
- Check if audio is muted in browser
- Try different browser

#### Issue 4: Language Not Supported
**Symptoms**: English works but Tamil/Hindi don't
**Solution**:
- This is normal - not all browsers support all languages
- Try Chrome (best support)
- Use English as fallback

#### Issue 5: Permission Denied
**Symptoms**: Console shows "Permission denied" error
**Solution**:
- Click "Allow" when browser asks for microphone/audio permission
- Check browser settings for audio permissions
- Try incognito mode

### Step 4: Browser-Specific Solutions

#### Chrome/Edge (Recommended)
- Best voice support
- Supports Tamil and Hindi
- Clear cache if issues persist

#### Firefox
- Good English support
- Limited Tamil/Hindi support
- May need to enable speech synthesis

#### Safari
- Limited support
- May not work at all
- Use Chrome instead

### Step 5: System-Level Checks

#### Windows
1. **Check Windows Audio**: Right-click speaker icon → "Open Sound settings"
2. **Check App Volume**: Settings → System → Sound → App volume
3. **Check Browser Volume**: Look for speaker icon in browser tab

#### Mac
1. **Check System Audio**: System Preferences → Sound
2. **Check Browser Audio**: Safari/Chrome audio settings
3. **Check Permissions**: System Preferences → Security & Privacy → Microphone

### Step 6: Advanced Debugging

If still not working, check these in console:

```javascript
// Test 1: Basic speech synthesis
if ('speechSynthesis' in window) {
  console.log('✅ Speech synthesis supported');
  const voices = window.speechSynthesis.getVoices();
  console.log('Voices:', voices.length);
} else {
  console.log('❌ Speech synthesis not supported');
}

// Test 2: Direct speech test
const utterance = new SpeechSynthesisUtterance('Test');
utterance.onstart = () => console.log('✅ Speech started');
utterance.onerror = (e) => console.log('❌ Error:', e.error);
window.speechSynthesis.speak(utterance);
```

### Step 7: Fallback Solutions

If voice alerts don't work:

1. **Use Visual Alerts**: The text alerts are still working
2. **Use Different Browser**: Try Chrome or Edge
3. **Use Different Device**: Try on phone/tablet
4. **Check Network**: Some browsers need internet for voices

## Expected Results

### ✅ Working Correctly
- Console shows: "Speech synthesis supported"
- Console shows: "Available Voices: X" (where X > 0)
- English speech works
- Console shows: "Speech started" and "Speech ended"

### ⚠️ Partially Working
- English works but Tamil/Hindi don't
- This is normal for some browsers
- Use English as fallback

### ❌ Not Working
- No speech synthesis support
- No voices available
- Permission denied errors
- Try different browser or device

## Contact Support

If none of these solutions work:
1. **Screenshot** the console errors
2. **Note** your browser and version
3. **Try** on different device/browser
4. **Report** the specific error messages

