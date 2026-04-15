# Pertemuan 6: Vue.js 3 Fundamentals — Composition API & Components

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Memahami Vue.js 3 architecture dan reactive system
- Menggunakan Composition API (ref, reactive, computed, watch)
- Membuat reusable components dengan props dan emits
- Mengelola component lifecycle hooks
- Menggunakan template directives (v-if, v-for, v-model, v-bind)
- Membuat forms dengan two-way data binding
- Implementasi component composition pattern

## 2. Pengantar: Hook

Vue.js adalah JavaScript framework untuk membuat interactive user interfaces. Di Gojek, Vue.js power aplikasi web driver dan customer — handling millions of interactions per day.

Vue 3 mengubah cara menulis component dengan **Composition API** — lebih fleksibel, reusable, dan scalable dibanding Options API. Kalau di Vue 2 logic tersebar di berbagai property (data, methods, computed), Vue 3 memungkinkan group logic by feature.

UI bukan hanya "display data". UI harus responsive, interactive, dan handle real-time updates. Vue reactivity system membuat ini seamless.

## 3. Konsep Utama

### 3.1 Reactivity System

```javascript
// Reactive value with ref
const count = ref(0);
console.log(count.value); // 0
count.value++; // 1
// When displayed in template, .value is automatic

// Reactive object with reactive()
const user = reactive({
    name: 'Budi',
    email: 'budi@gojek.com'
});
user.name = 'Rani'; // automatically triggers re-render

// Difference:
// ref() → best for primitives (string, number, boolean)
// reactive() → best for objects
```

### 3.2 Composition API Structure

```javascript
import { ref, reactive, computed, watch, onMounted } from 'vue';

export default {
    setup() {
        // Reactive data
        const count = ref(0);
        const user = reactive({ name: 'Budi' });
        
        // Computed (derived state)
        const double = computed(() => count.value * 2);
        
        // Methods
        const increment = () => count.value++;
        
        // Watchers
        watch(() => user.name, (newVal) => {
            console.log(`Name changed to ${newVal}`);
        });
        
        // Lifecycle
        onMounted(() => {
            console.log('Component mounted');
        });
        
        // Return exposed to template
        return { count, user, double, increment };
    }
}
```

### 3.3 Component Lifecycle

```
Setup → onBeforeMount → onMounted
        (component in DOM)
        ↓
        onBeforeUpdate → onUpdated
        (reactive data changed)
        ↓
        onBeforeUnmount → onUnmounted
        (component destroyed)
```

## 4. Ilustrasi & Analogi

**Analogi: Concert System**

- **ref/reactive:** Fan list (reactive state)
- **computed:** Total ticket revenue (derived from ticket prices)
- **watch:** Notify security when VIP fans arrive (side effects)
- **onMounted:** Set up sound system when concert starts
- **v-for:** Display each fan in list
- **v-if:** Show VIP section only if has VIP tickets

## 5. Contoh Teknis

### 5.1 Basic Component with Composition API

```javascript
// src/components/Counter.vue
<template>
    <div class="counter">
        <h2>Count: {{ count }}</h2>
        <p>Double: {{ double }}</p>
        
        <button @click="increment">Increment</button>
        <button @click="decrement">Decrement</button>
    </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
    name: 'Counter',
    setup() {
        const count = ref(0);
        
        const double = computed(() => count.value * 2);
        
        const increment = () => count.value++;
        const decrement = () => count.value--;
        
        return { count, double, increment, decrement };
    }
}
</script>

<style scoped>
.counter { padding: 20px; border: 1px solid #ccc; }
button { margin: 5px; padding: 8px 16px; }
</style>
```

### 5.2 Props & Emits (Parent-Child Communication)

```javascript
// src/components/UserCard.vue
<template>
    <div class="user-card">
        <h3>{{ user.name }}</h3>
        <p>Email: {{ user.email }}</p>
        <p>Role: {{ user.role }}</p>
        
        <button @click="makeAdmin" v-if="user.role !== 'admin'">
            Make Admin
        </button>
        <button @click="deleteUser" class="danger">Delete</button>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    name: 'UserCard',
    props: {
        user: {
            type: Object,
            required: true,
            validator: (u) => u.id && u.name && u.email
        }
    },
    emits: ['update-role', 'delete-user'],
    setup(props, { emit }) {
        const makeAdmin = () => {
            emit('update-role', props.user.id, 'admin');
        };
        
        const deleteUser = () => {
            if (confirm('Sure?')) {
                emit('delete-user', props.user.id);
            }
        };
        
        return { makeAdmin, deleteUser };
    }
}
</script>

// Parent component usage
<template>
    <div>
        <UserCard 
            v-for="user in users"
            :key="user.id"
            :user="user"
            @update-role="handleRoleUpdate"
            @delete-user="handleDelete"
        />
    </div>
</template>

<script>
export default {
    setup() {
        const users = ref([]);
        
        const handleRoleUpdate = (userId, newRole) => {
            // Update user role
        };
        
        const handleDelete = (userId) => {
            users.value = users.value.filter(u => u.id !== userId);
        };
        
        return { users, handleRoleUpdate, handleDelete };
    }
}
</script>
```

### 5.3 Forms & Two-Way Binding

```javascript
// src/components/LoginForm.vue
<template>
    <form @submit.prevent="handleSubmit" class="login-form">
        <div class="form-group">
            <label>Email:</label>
            <input 
                v-model="form.email"
                type="email"
                required
            />
            <span v-if="errors.email" class="error">{{ errors.email }}</span>
        </div>
        
        <div class="form-group">
            <label>Password:</label>
            <input 
                v-model="form.password"
                type="password"
                required
            />
        </div>
        
        <div class="form-group">
            <label>Remember me:</label>
            <input v-model="form.rememberMe" type="checkbox" />
        </div>
        
        <button type="submit" :disabled="isLoading">
            {{ isLoading ? 'Logging in...' : 'Login' }}
        </button>
    </form>
</template>

<script>
import { ref, reactive } from 'vue';

export default {
    name: 'LoginForm',
    emits: ['login'],
    setup(props, { emit }) {
        const form = reactive({
            email: '',
            password: '',
            rememberMe: false
        });
        
        const isLoading = ref(false);
        const errors = reactive({});
        
        const validateForm = () => {
            errors.email = '';
            
            if (!form.email.includes('@')) {
                errors.email = 'Invalid email';
                return false;
            }
            
            if (form.password.length < 8) {
                errors.password = 'Min 8 characters';
                return false;
            }
            
            return true;
        };
        
        const handleSubmit = async () => {
            if (!validateForm()) return;
            
            isLoading.value = true;
            try {
                emit('login', {
                    email: form.email,
                    password: form.password,
                    rememberMe: form.rememberMe
                });
            } finally {
                isLoading.value = false;
            }
        };
        
        return { form, isLoading, errors, handleSubmit };
    }
}
</script>
```

### 5.4 Computed Properties & Watchers

```javascript
// src/components/OrderSummary.vue
<script>
import { ref, computed, watch } from 'vue';

export default {
    setup() {
        const items = ref([
            { id: 1, name: 'Laptop', price: 10000000, qty: 1 },
            { id: 2, name: 'Mouse', price: 250000, qty: 2 }
        ]);
        
        const discountPercent = ref(0);
        
        // Computed: automatically update when items/discount change
        const subtotal = computed(() => {
            return items.value.reduce((sum, item) => 
                sum + (item.price * item.qty), 0
            );
        });
        
        const discount = computed(() => {
            return subtotal.value * (discountPercent.value / 100);
        });
        
        const total = computed(() => {
            return subtotal.value - discount.value;
        });
        
        // Watch: perform side effect when value changes
        watch(
            () => discountPercent.value,
            (newVal, oldVal) => {
                if (newVal > 50) {
                    alert('Max discount is 50%');
                    discountPercent.value = 50;
                }
            }
        );
        
        // Watch multiple sources
        watch(
            [() => items.value.length, () => subtotal.value],
            ([itemCount, subtotalVal]) => {
                console.log(`${itemCount} items, total: ${subtotalVal}`);
            }
        );
        
        return { items, discountPercent, subtotal, discount, total };
    }
}
</script>

<template>
    <div class="order-summary">
        <h3>Order Summary</h3>
        <p>Subtotal: Rp{{ subtotal }}</p>
        <p>Discount: Rp{{ discount }}</p>
        <p><strong>Total: Rp{{ total }}</strong></p>
    </div>
</template>
```

### 5.5 Lifecycle Hooks Example

```javascript
// src/components/DataTable.vue
<script>
import { ref, onMounted, onUpdated, onUnmounted } from 'vue';

export default {
    setup() {
        const data = ref([]);
        const isLoading = ref(true);
        let fetchInterval = null;
        
        // Called once when component mounts
        onMounted(async () => {
            console.log('Component mounted');
            
            // Fetch initial data
            const response = await fetch('/api/data');
            data.value = await response.json();
            isLoading.value = false;
            
            // Set up polling
            fetchInterval = setInterval(async () => {
                const response = await fetch('/api/data');
                data.value = await response.json();
            }, 30000); // refresh every 30 seconds
        });
        
        // Called when reactive data changes
        onUpdated(() => {
            console.log('Component updated, rerender data:', data.value);
        });
        
        // Called when component is destroyed
        onUnmounted(() => {
            console.log('Component unmounted');
            // Clean up: clear interval
            if (fetchInterval) {
                clearInterval(fetchInterval);
            }
        });
        
        return { data, isLoading };
    }
}
</script>

<template>
    <div v-if="isLoading" class="loading">Loading...</div>
    <table v-else>
        <tbody>
            <tr v-for="item in data" :key="item.id">
                <td>{{ item.name }}</td>
                <td>{{ item.value }}</td>
            </tr>
        </tbody>
    </table>
</template>
```

## 6. Studi Kasus Nyata: Gojek Driver Rating Component

Gojek shows real-time driver ratings. Component harus:
- Display rating stars
- Update on each trip completion
- Handle live notifications
- Smooth animations

```javascript
// src/components/DriverRating.vue
<template>
    <div class="driver-rating">
        <div class="stars">
            <span 
                v-for="i in 5" 
                :key="i"
                :class="['star', { filled: i <= rating }]"
                @click="setRating(i)"
            >
                ★
            </span>
        </div>
        
        <p v-if="rating" class="rating-text">
            {{ rating }}/5 ({{ reviewCount }} reviews)
        </p>
        
        <button 
            @click="submitRating" 
            :disabled="!rating || isSubmitting"
        >
            {{ isSubmitting ? 'Submitting...' : 'Submit Rating' }}
        </button>
    </div>
</template>

<script>
import { ref, computed, watch, onMounted } from 'vue';

export default {
    props: {
        driverId: {
            type: Number,
            required: true
        }
    },
    emits: ['rating-submitted'],
    setup(props, { emit }) {
        const rating = ref(0);
        const reviewCount = ref(0);
        const isSubmitting = ref(false);
        
        const submitRating = async () => {
            isSubmitting.value = true;
            try {
                const response = await fetch(
                    `/api/drivers/${props.driverId}/ratings`,
                    {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ score: rating.value })
                    }
                );
                
                if (response.ok) {
                    emit('rating-submitted', rating.value);
                    rating.value = 0;
                }
            } finally {
                isSubmitting.value = false;
            }
        };
        
        const setRating = (r) => {
            rating.value = r;
        };
        
        onMounted(async () => {
            // Load driver's current rating
            const response = await fetch(
                `/api/drivers/${props.driverId}/rating`
            );
            const data = await response.json();
            reviewCount.value = data.reviewCount;
        });
        
        return { rating, reviewCount, isSubmitting, submitRating, setRating };
    }
}
</script>

<style scoped>
.stars { font-size: 2em; cursor: pointer; }
.star { color: #ddd; }
.star.filled { color: #ffc107; }
</style>
```

## 7. Visualisasi: Component Lifecycle

```
Create
  ↓
setup() ← reactive data, methods
  ↓
onBeforeMount()
  ↓
MOUNT → Render to DOM
  ↓
onMounted() ← fetch data, set timers
  ↓
Reactive data changes
  ↓
onBeforeUpdate()
  ↓
UPDATE → Re-render
  ↓
onUpdated() ← post-render logic
  ↓
(repeat Mount/Update cycle)
  ↓
Component destroyed
  ↓
onBeforeUnmount()
  ↓
UNMOUNT → Remove from DOM
  ↓
onUnmounted() ← cleanup (timers, etc)
```

## 8. Kesalahan Umum

### ❌ Mutating reactive objects directly in template

```javascript
// ❌ WRONG - template modifies reactive state
<input @input="count = $event.target.value" />

// ✅ CORRECT - use v-model or method
<input v-model="count" />
// or
<input @input="count = parseInt($event.target.value)" />
```

### ❌ Not returning from setup()

```javascript
// ❌ WRONG - variables not accessible in template
export default {
    setup() {
        const count = ref(0);
        // forgot return!
    }
}

// ✅ CORRECT
export default {
    setup() {
        const count = ref(0);
        return { count }; // must return
    }
}
```

### ❌ Heavy computation in computed without dependency

```javascript
// ❌ SLOW - runs every time template renders
const sorted = computed(() => {
    console.log('Sorting...');
    return largeArray.sort(); // O(n log n) every time
});

// ✅ OPTIMIZED - sort only when source changes
const sorted = computed(() => {
    return [...items.value].sort(); // create copy, sort once
});
```

### ❌ Memory leak in onMounted

```javascript
// ❌ WRONG - timer not cleaned up
onMounted(() => {
    setInterval(() => {
        // update data
    }, 1000);
});

// ✅ CORRECT - clean up in onUnmounted
onMounted(() => {
    const id = setInterval(() => {
        // update data
    }, 1000);
});

onUnmounted(() => {
    clearInterval(id);
});
```

## 9. Latihan & Studi Kasus

### Latihan 1: Counter Component
```javascript
// Task: Create counter with Composition API
// - Display count
// - Increment/Decrement buttons
// - Show double in separate element
```

**Solusi:**
```javascript
export default {
    setup() {
        const count = ref(0);
        const double = computed(() => count.value * 2);
        return {
            count,
            double,
            increment: () => count.value++,
            decrement: () => count.value--
        };
    }
}
```

### Latihan 2: Form with Validation
```javascript
// Task: Create form component
// - Two input fields (email, password)
// - Validate email format
// - Show error messages
// - Disable submit if invalid
```

**Solusi:**
```javascript
const form = reactive({ email: '', password: '' });
const errors = reactive({});

const isValid = computed(() => 
    form.email.includes('@') && form.password.length >= 8
);

const validate = () => {
    errors.email = form.email.includes('@') ? '' : 'Invalid email';
    errors.password = form.password.length >= 8 ? '' : 'Min 8 chars';
};
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami Vue 3 reactivity (ref vs reactive)
- [ ] Bisa membuat Composition API components
- [ ] Bisa menggunakan computed properties
- [ ] Bisa menggunakan watchers
- [ ] Bisa implement props dan emits
- [ ] Bisa use lifecycle hooks
- [ ] Bisa bind forms dengan v-model
- [ ] Bisa render lists dengan v-for
- [ ] Bisa kondisional render dengan v-if
- [ ] Bisa cleanup resources (memory leaks)

## 11. Referensi

- [Vue.js 3 Documentation](https://vuejs.org)
- [Composition API](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Vue Style Guide](https://vuejs.org/style-guide/)

**Status:** ✅ Pertemuan 6 selesai
