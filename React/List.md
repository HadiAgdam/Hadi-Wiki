
```jsx
const [students, setStudents] = useState([
    { id: 1, name: "Hadi", grade: 18.5 },
    { id: 2, name: "Ali", grade: 14 }
]);
```

Add item:
```jsx
setStudents(current => [
    ...current,
    { id: 3, name: "Sara", grade: 19.25 }
]);
```