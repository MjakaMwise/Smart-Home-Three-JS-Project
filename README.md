# 3D House Control System

A real-time 3D smart home visualization and control system built with Three.js and Firebase. Control house elements like curtains, fans, and lighting from any device with seamless synchronization.

## 🌟 Features

### 3D Visualization
- **Realistic 3D Environment**: Fully rendered sitting room with detailed models
- **Multiple Camera Views**: Front, side, top, back, and custom setting views
- **Day/Night Mode**: Toggle between day and night lighting with ambient adjustments
- **Smooth Transitions**: Camera movements with easing animations

### Interactive Controls
- **Curtain Control**: Open/close three independent curtain systems
- **Ceiling Fan**: Toggle on/off with speed control and acceleration
- **Lighting System**: Toggle lamps with realistic spotlight and glow effects
- **Real-time Sync**: Control from mobile device, see changes on desktop viewer

### Smart Architecture
- **Device Detection**: Automatic redirect to control panel (mobile) or viewer (desktop)
- **Firebase Integration**: Real-time database for instant command propagation
- **CSG Operations**: Advanced wall construction with window cutouts
- **Shadow Mapping**: Realistic shadow rendering for all objects

## 🚀 Quick Start

### Prerequisites
```
- Modern web browser (Chrome, Firefox, Edge, Safari)
- Firebase account (for backend)
- Local web server (for testing)
```

### Installation

1. **Clone/Download the project files**
   ```bash
   # Your project structure should look like:
   project/
   ├── index.html       # Device detector & router
   ├── viewer.html      # 3D visualization (desktop)
   ├── control.html     # Control panel (mobile)
   └── models/          # 3D model files
       ├── kiti.glb
       ├── curtain.glb
       ├── fan.glb
       └── ...
   ```

2. **Set up Firebase**
   - Create a new Firebase project at [firebase.google.com](https://firebase.google.com)
   - Enable Realtime Database
   - Replace the Firebase config in all HTML files:
   ```javascript
   const firebaseConfig = {
       apiKey: "YOUR_API_KEY",
       authDomain: "YOUR_PROJECT.firebaseapp.com",
       databaseURL: "https://YOUR_PROJECT.firebaseio.com",
       projectId: "YOUR_PROJECT_ID",
       storageBucket: "YOUR_PROJECT.appspot.com",
       messagingSenderId: "YOUR_SENDER_ID",
       appId: "YOUR_APP_ID"
   };
   ```

3. **Set up Firebase Database Rules**
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```

4. **Deploy/Run**
   ```bash
   # Option 1: Use a local server
   python -m http.server 8000
   
   # Option 2: Use Live Server (VS Code extension)
   # Option 3: Deploy to Firebase Hosting, Netlify, or Vercel
   ```

5. **Access the application**
   - Open `index.html` in your browser
   - On mobile: You'll see the control panel
   - On desktop: You'll see the 3D viewer

## 📱 Usage

### Control Panel (Mobile)
1. Open the app on your smartphone
2. Tap any button to control house elements
3. Changes appear instantly on the viewer

### Available Controls
- **FrontView / SideView / TopView / BackView**: Change camera angle
- **Open/Close Window 1, 2, 3**: Control curtains
- **Toggle Fan**: Start/stop ceiling fan
- **Day Toggle**: Switch between day and night mode

### 3D Viewer (Desktop)
- **Mouse Controls**:
  - Left click + drag: Rotate view
  - Right click + drag: Pan
  - Scroll wheel: Zoom in/out
- **Keyboard Shortcuts**: (Can be added)
- **Camera Views**: Click buttons in control panel to switch

## 🔧 Customization

### Adding New Buttons
```javascript
// In control.html, add to Firebase:
const buttonsRef = ref(db, 'buttons');
set(buttonsRef, {
  newButton: {
    name: "Custom Action",
    code: "window.myCustomFunction();"
  }
});
```

### Adding New 3D Models
```javascript
// In viewer.html:
T.addGLBModel("your-model.glb", {
  position: [x, y, z],
  scale: 100,
  name: "myModel"
}).then(({ model, animations }) => {
  console.log("Model loaded!");
  window.myModel = model;
});
```

### Exposing New Functions
```javascript
// Add to window object for remote control:
window.myCustomFunction = () => {
  // Your code here
  myObject.position.y += 1;
};
```

## 🏗️ Architecture

### Device Detection Flow
```
index.html (Device Detector)
     ↓
Smartphone? → control.html (Control Panel)
Desktop?    → viewer.html (3D Viewer)
```

### Communication Flow
```
Control Panel (Mobile)
     ↓ [Button Press]
Firebase Realtime Database
     ↓ [Trigger Event]
3D Viewer (Desktop)
     ↓ [Execute Code]
Visual Update
```

## 📚 API Reference

### Scene Manager Functions

#### Camera Controls
```javascript
window.switchToFrontView()      // Front camera view
window.switchToSideViewR()      // Right side view
window.switchToTopView()        // Top-down view
window.switchToSettingView()    // Custom setting view
```

#### Lighting
```javascript
window.toggleDayNight()                    // Toggle day/night
window.setAmbientLight(intensity)          // 0.0 to 1.0
window.setPointLightPosition(x, y, z)      // Move point light
window.setPointLightIntensity(intensity)   // Light brightness
```

#### Object Controls
```javascript
window.openCurtain1()           // Open first curtain
window.closeCurtain1()          // Close first curtain
window.toggleFan()              // Toggle ceiling fan
window.setFanSpeed(speed)       // 0.5 = slow, 2 = fast
window.lampAOn()                // Turn lamp on
window.lampAOff()               // Turn lamp off
```

#### Utilities
```javascript
window.logCameraPosition()      // Print camera coords
window.listAllObjects()         // Show all scene objects
window.clearAllObjects()        // Remove all objects
```

## 🛠️ Technologies Used

- **Three.js** (r170) - 3D rendering engine
- **Firebase** - Real-time database & hosting
- **three-bvh-csg** - Boolean operations for walls
- **GLTFLoader** - 3D model loading
- **OrbitControls** - Camera interaction

## 📦 3D Models Required

Place these `.glb` files in your project root:
- `kiti.glb` - Sofa model 1
- `kiti2.glb` - Sofa model 2
- `curtain.glb` - Window curtain
- `curtain2.glb` - Back curtain
- `fan.glb` - Ceiling fan
- `lamp.glb` - Table lamp
- `dinning.glb` - Dining table
- `tv.glb` - Television
- `grass.glb` - Ground texture
- `glasstable.glb` - Glass coffee table

## 🐛 Troubleshooting

### Models Not Loading
- Check file paths match exactly (case-sensitive)
- Ensure `.glb` files are in correct directory
- Check browser console for 404 errors

### Firebase Errors
- Verify Firebase config matches your project
- Check database rules allow read/write
- Ensure network connection is active

### Controls Not Working
- Check `window` functions are exposed after model loads
- Verify Firebase connection in control panel
- Look for JavaScript errors in console

## 🤝 Contributing

Contributions welcome! Please follow these steps:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👥 Authors

- **Marshall Israel -MjakaMWise** - Initial work

## 🙏 Acknowledgments

- Three.js community for excellent documentation
- Firebase for real-time infrastructure
- All 3D model creators

## 📞 Support

For issues or questions:
- Open an issue on GitHub

---

**Made with ❤️ by MjakaMwise**
