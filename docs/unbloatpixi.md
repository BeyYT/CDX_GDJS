# UNBLOAT PART
## As You Know, Pixi.js is an Renderer For GDevelop, However, in Some Instances....
![example1](https://media.discordapp.net/attachments/948281490788192279/954137440841519194/unknown.png)

Why Is WebGL an Legacy Mode?

![example2](https://media.discordapp.net/attachments/948281490788192279/954137764176216144/unknown.png)

Why is there an unkown/unused WebGL Mode?

![example3](https://cdn.discordapp.com/attachments/948281490788192279/954138729524637726/unknown.png)

Half of All GDevelop Games Use At Least 20% of this.

However Due To Obvious Reasons, We Cannot Remove This, What We Can Remove..

Debug Code Launching In RuntimeScene:
```javascript
renderDebugDraw(r, a, c, u) {
            (!this._debugDraw || !this._debugDrawContainer) &&
                ((this._debugDrawContainer = new g.Container()), (this._debugDraw = new g.Graphics()), this._debugDrawContainer.addChild(this._debugDraw), this._pixiContainer.addChild(this._debugDrawContainer));
            const t = this._debugDraw;
            for (let n in this._debugDrawRenderedObjectsPoints) this._debugDrawRenderedObjectsPoints[n].wasRendered = !1;
            const _ = (n, e, i, h, o) => {
                (t.line.color = i), (t.fill.color = i), t.drawCircle(h, o, 3), c && (n[e] || ((n[e] = new g.Text(e, { fill: i, fontSize: 12 })), this._debugDrawContainer.addChild(n[e])), n[e].position.set(h, o));
            };
            t.clear(), t.beginFill(), (t.alpha = 0.8), t.lineStyle(2, 255, 1);
            for (let n = 0; n < r.length; n++) {
                const e = r[n],
                    i = this._runtimeScene.getLayer(e.getLayer());
                if (((!e.isVisible() || !i.isVisible()) && !a) || !e.getRendererObject()) continue;
                const o = e.getAABB();
                (t.fill.alpha = 0.2), (t.line.color = 7835368), (t.fill.color = 7835368);
                const s = [];
                s.push.apply(s, i.convertInverseCoords(o.min[0], o.min[1])),
                    s.push.apply(s, i.convertInverseCoords(o.max[0], o.min[1])),
                    s.push.apply(s, i.convertInverseCoords(o.max[0], o.max[1])),
                    s.push.apply(s, i.convertInverseCoords(o.min[0], o.max[1])),
                    t.drawPolygon(s);
            }
            for (let n = 0; n < r.length; n++) {
                const e = r[n],
                    i = this._runtimeScene.getLayer(e.getLayer());
                if (((!e.isVisible() || !i.isVisible()) && !a) || !e.getRendererObject()) continue;
                const o = e.id;
                this._debugDrawRenderedObjectsPoints[o] || (this._debugDrawRenderedObjectsPoints[o] = { wasRendered: !0, points: {} });
                const s = this._debugDrawRenderedObjectsPoints[o];
                s.wasRendered = !0;
                const x = e.getHitBoxes();
                for (let d = 0; d < x.length; d++) {
                    const l = [];
                    x[d].vertices.forEach((f) => {
                        (f = i.convertInverseCoords(f[0], f[1])), l.push(f[0]), l.push(f[1]);
                    }),
                        (t.fill.alpha = 0),
                        (t.line.alpha = 0.5),
                        (t.line.color = 16711680),
                        t.drawPolygon(l);
                }
                t.fill.alpha = 0.3;
                const R = i.convertInverseCoords(e.getCenterXInScene(), e.getCenterYInScene());
                _(s.points, "Center", 16776960, R[0], R[1]);
                const p = i.convertInverseCoords(e.getX(), e.getY());
                if ((_(s.points, "Position", 16711680, p[0], p[1]), e instanceof b.SpriteRuntimeObject)) {
                    let d = e.getPointPosition("origin");
                    (Math.abs(d[0] - p[0]) >= 1 || Math.abs(d[1] - p[1]) >= 1) && ((d = i.convertInverseCoords(d[0], d[1])), _(s.points, "Origin", 16711680, d[0], d[1]));
                }
                if (u && e instanceof b.SpriteRuntimeObject) {
                    if (!e._animationFrame) continue;
                    for (const d in e._animationFrame.points.items) {
                        let l = e.getPointPosition(d);
                        (l = i.convertInverseCoords(l[0], l[1])), _(s.points, d, 255, l[0], l[1]);
                    }
                }
            }
            for (const n in this._debugDrawRenderedObjectsPoints) {
                const e = this._debugDrawRenderedObjectsPoints[n];
                if (e.wasRendered) continue;
                const i = e.points;
                for (const h in i) this._debugDrawContainer.removeChild(i[h]);
            }
            t.endFill();
        }
        clearDebugDraw() {
            this._debugDraw && this._debugDraw.clear(),
                this._debugDrawContainer && (this._debugDrawContainer.destroy({ children: !0 }), this._pixiContainer.removeChild(this._debugDrawContainer)),
                (this._debugDraw = null),
                (this._debugDrawContainer = null),
                (this._debugDrawRenderedObjectsPoints = {});
        }
```

Electron + opening url's (safe mode)
```javascript
// Code up here
        openURL(n) {
            if (typeof window != "undefined") {
                const e = this.getElectron();
                if (e) e.shell.openExternal(n);
                else {
                    const t = window.cordova ? "_system" : "_blank";
                    window.open(n, t);
                }
            }
        }
// Code Down Here....
        getElectron() {
            return typeof require != "undefined" ? require("electron") : null;
        }
// like 4 more lines of code
```

Helps Quite A Bit.

# SPEED UP / VSYNC PART
Not Only That, but in the pixi.js file we find this flag:
```javascript
// line 3415 - 3434
        Tr = (function () {
            function e() {
                var t = this;
                (this.autoStart = !1),
                    (this.deltaTime = 1),
                    (this.lastTime = -1),
                    (this.speed = 1),
                    (this.started = !1),
                    (this._requestId = null),
                    (this._maxElapsedMS = 100),
                    (this._minElapsedMS = 0),
                    (this._protected = !1),
                    (this._lastFrame = -1),
                    (this._head = new Er(null, null, 1 / 0)),
                    (this.deltaMS = 1 / rt.TARGET_FPMS),
                    (this.elapsedMS = 1 / rt.TARGET_FPMS),
                    (this._tick = function (e) {
                        (t._requestId = null), t.started && (t.update(e), t.started && null === t._requestId && t._head.next && (t._requestId = requestAnimationFrame(t._tick)));
                    });
            }
//line 17065 - 17084
        xl = (function () {
            function t() {
                var t = this;
                (this.autoStart = !1),
                    (this.deltaTime = 1),
                    (this.lastTime = -1),
                    (this.speed = 1),
                    (this.started = !1),
                    (this._requestId = null),
                    (this._maxElapsedMS = 100),
                    (this._minElapsedMS = 0),
                    (this._protected = !1),
                    (this._lastFrame = -1),
                    (this._head = new Tl(null, null, 1 / 0)),
                    (this.deltaMS = 1 / rt.TARGET_FPMS),
                    (this.elapsedMS = 1 / rt.TARGET_FPMS),
                    (this._tick = function (e) {
                        (t._requestId = null), t.started && (t.update(e), t.started && null === t._requestId && t._head.next && (t._requestId = requestAnimationFrame(t._tick)));
                    });
            }
```

Do You See It Yet?

No? lemme show you the lines.
```javascript
// line 3415 - 3434
                    (this.deltaTime = 1),
                    (this.lastTime = -1),
                    (this.speed = 1),
                    (this._maxElapsedMS = 100),
                    (this._minElapsedMS = 0),
                    (this.deltaMS = 1 / rt.TARGET_FPMS),
                    (this.elapsedMS = 1 / rt.TARGET_FPMS),
//line 17065 - 17084
                    (this.deltaTime = 1),
                    (this.lastTime = -1),
                    (this.speed = 1),
                    (this._maxElapsedMS = 100),
                    (this._minElapsedMS = 0),
                    (this.deltaMS = 1 / rt.TARGET_FPMS),
                    (this.elapsedMS = 1 / rt.TARGET_FPMS),
```

Yes, That is indeed right. you can edit the delta time, speed of game, and the target frames per ms.

by default, the target is this:

```javascript
// line 3387
(rt.TARGET_FPMS = 0.06),
//line 17036
(rt.TARGET_FPMS = 0.06),
```
But You Can Change it to 0.05, 0.04, 0.03, 0.02 and 0.01, which should be able to at lease try and make the game smoother.

In This Test, We Changed it to 0.01

Here is an test electron build (with and without the fix.)

Main.js:
```javascript
/**
 * This is the file handling the startup and lifetime of the game
 * running in Electron Runtime.
 */
// Modules to control application life and create native browser window
const { app, BrowserWindow, shell, Menu } = require("electron");

// Keep a global reference of the window object, if you don't, the window will
// be closed automatically when the JavaScript object is garbage collected.
let mainWindow = null;

function createWindow() {
  // Create the browser window.
  mainWindow = new BrowserWindow({
    width: 1366,
    height: 768,
    useContentSize: true,
    title: "test",
    backgroundColor: '#000000',
    webPreferences: {
      nodeIntegration: true,
    }
  });

  // Open external link in the OS default browser
  mainWindow.webContents.on("new-window", function(e, url) {
    e.preventDefault();
    shell.openExternal(url);
  });

  // and load the index.html of the app.
  mainWindow.loadFile("app/index.html");

  Menu.setApplicationMenu(null);

  // Open the DevTools.
  // mainWindow.webContents.openDevTools()

  // Emitted when the window is closed.
  mainWindow.on("closed", function() {
    // Dereference the window object, usually you would store windows
    // in an array if your app supports multi windows, this is the time
    // when you should delete the corresponding element.
    mainWindow = null;
  });
}

// Should be set to true, which will be the default value in future Electron
// versions, but then causes an issue on Windows where the `fs` module stops	
// working in the renderer process after a reload.
// See https://github.com/electron/electron/issues/22119
// For now, out of caution, disable this as we rely heavily on `fs` in the 
// renderer process.
app.allowRendererProcessReuse = false;

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.commandLine.appendSwitch('disable-frame-rate-limit')
app.on("ready", createWindow);

// Quit when all windows are closed.
app.on("window-all-closed", function() {
  app.quit();
});
```

Test With Fix:


https://user-images.githubusercontent.com/60990459/158907381-4b7f9fa9-eae0-4bd0-aefa-751e0f8c5bcb.mp4

Test Without Fix:


https://user-images.githubusercontent.com/60990459/158907403-1ed988af-8f48-4186-a4a5-d414d5913fa2.mp4


And Thats It.

p.s if your data.js has this:
```javascript
"verticalSync":true,
```

change it to:
```javascript
"verticalSync":false,
```

[Download Electron Build Fix](https://cdn.discordapp.com/attachments/948281490788192279/954153211361628180/fix_electron.zip)

[Download HTML5 Fix](https://cdn.discordapp.com/attachments/948281490788192279/954153526966226954/fix_html5.zip)
