# ScreenshotGUIAppInGithubAction

- Runs a gui command using Xvfb (virtual framebuffer) 
- Extracts a screenshot using xwd (x window dump)
- Artifacts the screenshot

Quite useful for quickly testing a GUI app in CI with pictures

```
jobs:
  linuxRun:
    runs-on: ubuntu-18.04
    
    steps:
      - name: Install dependencies
        run: sudo apt-get install -y xvfb x11-apps imagemagick xterm

      - name: launch and screenshot
        run: |
          export DISPLAY=:99
          sudo Xvfb :99 -screen 0 1024x768x8 &
          sleep 5
          xterm -hold -e echo "Checkout the artifact!" 2>/dev/null &
          sleep 5
          xwd -root -silent | convert xwd:- png:screenshot.png
          
      - name: upload artifact
        uses: actions/upload-artifact@v3
        with:
          name: screenshot
          path: screenshot.png
```

The sleeps appear necessary to allow for the app and frame buffer to run
### E.g

![screenshot](https://user-images.githubusercontent.com/84378622/222271538-e02c29ea-231e-465a-87c8-022cc622629d.png)
