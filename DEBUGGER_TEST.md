# 🐛 Debugger Configuration Test & Verification

## ✅ Configuration Summary

### Java Debugging Settings
```json
✓ Hot Code Replace: Disabled (stable debugging)
✓ Show toString(): Enabled
✓ Show Logical Structure: Enabled
✓ Max String Length: Unlimited (no truncation)
✓ Skip Synthetics: Enabled
✓ Skip Static Initializers: Enabled
✓ Skip Constructors: Disabled (can debug constructors)
✓ Console: Integrated Terminal
✓ Force Build Before Launch: Enabled
✓ Code Lens: Enabled
```

### General Debug Settings
```json
✓ Inline Values: Off (cleaner view)
✓ Auto Expand Lazy Variables: Off (performance)
✓ Show Breakpoints in Overview: Enabled
✓ Show Inline Breakpoint Candidates: Enabled
✓ Toolbar Location: Docked
✓ Console Close on End: False (keeps history)
✓ Console History Suggestions: Enabled
✓ Console Word Wrap: Enabled
✓ Focus Window on Break: Enabled
✓ Allow Breakpoints Everywhere: Enabled
```

### Remote Debugging
```json
✓ Timeouts: 60-120 seconds (reliable over slow networks)
✓ Java JDWP: Ports 5005, 30166, custom
✓ Python debugpy: Port 5678
✓ Node.js: Port 9229
✓ Docker: Dedicated configs with 120s timeout
```

### Performance Optimizations
```json
✓ Skip Classes: JDK, Spring, Logback, Reactor, etc.
✓ JVM Args: -XX:+UseZGC (low latency GC)
✓ Fast Launch: -XX:+TieredCompilation
✓ Memory: -Xms2g -Xmx8g for language server
```

---

## 🧪 Test Checklist

### Local Java Debugging
- [ ] **Launch Java App**: Press F5 → Select "Java: Launch Main (Fast)"
- [ ] **Set Breakpoint**: Click in gutter, red dot appears
- [ ] **Hit Breakpoint**: App pauses at breakpoint
- [ ] **Variables Panel**: Shows local variables, no truncation
- [ ] **Debug Console**: Can evaluate expressions
- [ ] **Step Over (F10)**: Steps to next line
- [ ] **Step Into (F11)**: Steps into method
- [ ] **Step Out (Shift+F11)**: Steps out of method
- [ ] **Continue (F5)**: Continues execution
- [ ] **Restart (Ctrl+Shift+F5)**: Restarts debug session
- [ ] **Stop (Shift+F5)**: Stops debugging

### Remote Java Debugging (JDWP)
- [ ] **Start Remote App**: `java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar app.jar`
- [ ] **Attach Debugger**: F5 → "Java: Remote Attach (JDWP 5005) Local"
- [ ] **Connection Success**: Shows "Attached to remote target" in debug console
- [ ] **Set Breakpoint**: In remote code
- [ ] **Hit Remote Breakpoint**: Pauses when remote code hits breakpoint
- [ ] **Inspect Variables**: Can see remote variables
- [ ] **Timeout Works**: 60s timeout prevents premature disconnection

### Python Debugging
- [ ] **Launch Python File**: F5 → "Python: Current File"
- [ ] **Remote Attach**: Start Python with debugpy, attach via "Python: Remote Attach"
- [ ] **Django**: Launch Django app via "Python: Django"

### Node.js Debugging
- [ ] **Launch Node File**: F5 → "Node: Launch Current File"
- [ ] **Remote Attach**: Start node with `--inspect=9229`, attach via "Node: Remote Attach"

### Docker Debugging
- [ ] **Java Container**: Attach to containerized Java app on port 5005
- [ ] **Node Container**: Attach to containerized Node app on port 9229
- [ ] **120s Timeout**: Sufficient time for container startup

### Performance & Reliability
- [ ] **Fast Stepping**: Skips framework classes (Spring, JDK, etc.)
- [ ] **No Lag**: Steps execute quickly (< 200ms)
- [ ] **Stable Connection**: Remote debugging doesn't disconnect randomly
- [ ] **Build Before Launch**: Always builds latest code
- [ ] **No Hot Reload Issues**: Disabled for stability

### Console & UI
- [ ] **Integrated Terminal**: Debug output shows in integrated terminal
- [ ] **Word Wrap**: Long lines wrap in debug console
- [ ] **History**: Up arrow shows previous console commands
- [ ] **Focus on Break**: Window focuses when breakpoint hit
- [ ] **Toolbar Docked**: Debug toolbar stays in fixed position

---

## 🎯 Launch Configurations Available

### Java
1. **Java: Launch Main (Fast)** - Quick launch with optimizations
2. **Java: Launch Main (with Remote Debug Port 5005)** - Launch + expose port
3. **Java: Launch Main (Debug Mode - Suspend on Start)** - Pause before main()
4. **Java: Remote Attach (JDWP 30166) TeamLabs** - Your TeamLabs setup
5. **Java: Remote Attach (JDWP 5005) Local** - Standard local attach
6. **Java: Remote Attach (Custom Host/Port)** - Interactive prompts
7. **Java: Run Tests (Maven)** - Run tests via Maven
8. **Java: Debug Test (Current)** - Debug single test
9. **Java: Coverage Report (JaCoCo)** - Generate coverage

### Python
1. **Python: Remote Attach (debugpy 5678)** - Remote Python debugging
2. **Python: Current File** - Quick file debugging
3. **Python: Django** - Django with debugging

### Node.js
1. **Node: Remote Attach (9229)** - Remote Node debugging
2. **Node: Launch Current File** - Quick file debugging

### Docker
1. **Docker: Attach to Java Container (5005)** - Java in Docker
2. **Docker: Attach to Node Container (9229)** - Node in Docker

### Compound (Multi-target)
1. **Java: Launch + Attach (Local JDWP)** - Launch and attach simultaneously
2. **Full Stack: Java + Node** - Debug both at once

---

## 🚀 Quick Test Commands

### Test Java Debugging
```bash
# Create test file
cat > /tmp/DebugTest.java << 'EOF'
public class DebugTest {
    public static void main(String[] args) {
        int x = 42;
        int y = 58;
        int sum = x + y;  // <- Set breakpoint here
        System.out.println("Sum: " + sum);
    }
}
EOF

# Compile and run
cd /tmp
javac DebugTest.java
java DebugTest
```

### Test Remote JDWP
```bash
# Start with debug port
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 DebugTest

# In VS Code: F5 → "Java: Remote Attach (JDWP 5005) Local"
```

### Test Python Remote Debugging
```bash
# Install debugpy
pip install debugpy

# Create test file
cat > /tmp/debug_test.py << 'EOF'
import debugpy
debugpy.listen(5678)
print("Waiting for debugger...")
debugpy.wait_for_client()

x = 42
y = 58
sum = x + y  # <- Set breakpoint here
print(f"Sum: {sum}")
EOF

# Run
python /tmp/debug_test.py

# In VS Code: F5 → "Python: Remote Attach (debugpy 5678)"
```

---

## 💡 Troubleshooting

### Issue: Breakpoints not hitting
- ✓ Check "Force Build Before Launch" is enabled
- ✓ Verify source code matches running code
- ✓ Ensure breakpoint is on executable line (not empty/comment)

### Issue: Slow stepping
- ✓ Check "Skip Classes" is configured
- ✓ Disable "Show toString()" if objects are huge
- ✓ Close unnecessary variables panels

### Issue: Remote connection fails
- ✓ Verify port is exposed (firewall, docker)
- ✓ Check timeout is sufficient (60-120s)
- ✓ Confirm remote app started with debug agent

### Issue: Variables truncated
- ✓ "Max String Length" should be 0 (unlimited)
- ✓ Check "Show Logical Structure" is enabled

---

## ✅ All Systems Go!

If all tests pass:
- **Local Debugging**: ✅ Fast, reliable, no lag
- **Remote Debugging**: ✅ Stable connections, generous timeouts
- **Performance**: ✅ Framework classes skipped, fast stepping
- **UI/UX**: ✅ Clean, word wrap, history, auto-focus

**Ready for production debugging!** 🎯
