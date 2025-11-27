# Voice Alert Testing Instructions

## How to Test Tamil/Hindi Voice Alerts

### Step 1: Open Settings
1. Navigate to http://localhost:8080 (or the port shown in terminal)
2. Click on **Settings** in the menu

### Step 2: Configure Voice Settings
1. Scroll to the **Voice** section
2. Toggle **"Enable voice alerts"** to ON (if not already on)
3. Select **Voice Language** dropdown
4. Choose one of:
   - **Tamil (தமிழ்)** 
   - **Hindi (हिंदी)**

### Step 3: Test Voice Alert
1. Go to **Map** page or **Dashboard** page
2. Click **Start Tracking** button
3. The system will simulate zone violations
4. You should see an alert appear
5. **Listen for the voice alert in your selected language**

### Step 4: Verify Console Logs
1. Open browser Developer Tools (F12)
2. Go to Console tab
3. You should see logs like:
   - "Settings saved: {voiceLanguage: 'tamil'}"
   - "Speaking alert: {text: 'அபாய எச்சரிக்கை', lang: 'tamil'}"
   - "Speech synthesis lang: ta-IN"

## Troubleshooting

If language not working, check console logs for detailed information about what language is being used.
