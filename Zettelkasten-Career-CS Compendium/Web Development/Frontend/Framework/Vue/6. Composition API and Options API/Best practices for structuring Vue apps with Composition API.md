---
tags:
  - Vue
---
The Composition API gives you a lot of flexibility—**but with that comes responsibility**. A well-structured Vue app is easier to scale, test, and maintain. Below are some of the best practices you should follow when using the Composition API in Vue 3\.

---

# Organize Logic into Composables

* Extract logic into **reusable functions** called **composables**.

* This makes your components cleaner and separates concerns.

**Example: `useCounter.js`**

```javascript
// composables/useCounter.js
import { ref } from 'vue'

export function useCounter() {
  const count = ref(0)
  const increment = () => count.value++
  return { count, increment }
}
```

**Use in a component:**

```vue
<script setup>
import { useCounter } from '@/composables/useCounter'
const { count, increment } = useCounter()
</script>
```

---

# Group Logic by Feature (Not by Type)

* Instead of splitting code by `data`, `methods`, etc., **group it by what it does**.

* For example: grouping `form` logic together, `API` logic together, etc.

```javascript
// Good in Composition API
const name = ref('')
const validateName = () => { /* ... */ }

const fetchUser = async () => { /* ... */ }

onMounted(() => {
  fetchUser()
})
```

---

# Use `<script setup>` for Simplicity

* It removes boilerplate and auto-exposes variables to the template.

* Keeps your components short and focused.

```vue
<script setup>
import { ref } from 'vue'
const name = ref('Alice')
</script>
```

---

# Define Global Composables for Common Features

* Examples: `useAuth()`, `useApi()`, `useFormValidation()`

* These can live in a `composables/` directory and be reused across components.

---

# Use `ref()` for primitives and `reactive()` for objects

* Follow Vue’s reactivity rules to avoid unnecessary bugs.

* If you want to destructure from `reactive()`, do it carefully (you’ll lose reactivity unless using `toRefs()`).

---

# Watchers Should Be Specific

* Avoid vague or deep watchers unless absolutely necessary.

* Use `watchEffect()` when you want reactive tracking without specifying dependencies.

---

# Keep Components Small and Focused

* Follow the **Single Responsibility Principle**.

* If your component does too much, break it down into smaller components or composables.

---

# Use TypeScript (Optional but Recommended)

* The Composition API works **really well with TypeScript**.

* It provides better type inference and safer code, especially with composables and props.

---

# Organize Files by Feature (Modular Structure)

Instead of:

```
components/
  UserCard.vue
  UserForm.vue
```

Use:

```
features/
  user/
    components/
      UserCard.vue
      UserForm.vue
    useUser.js
```

This keeps everything related to a specific feature or module in one place.

---

# Use Meaningful Names for Composables

* Prefix with `use` to indicate they are composable (like `useProductList()`, `useFormState()`).

* This improves clarity and helps with editor autocomplete.

---

# Recap

| Best Practice | Why It Matters |
| ----- | ----- |
| Use composables | Reusable, testable logic |
| Use `<script setup>` | Less boilerplate |
| Organize by feature | Better scalability |
| Group logic, not by type | Easier to read & maintain |
| Small, focused components | Cleaner UI structure |
| Use `ref`, `reactive` appropriately | Avoid reactivity bugs |
| Watch carefully | Reduce unexpected re-renders |
| Add TypeScript (optional) | Safer, more powerful code |
| Structure files clearly | Team-friendly and scalable |


