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
