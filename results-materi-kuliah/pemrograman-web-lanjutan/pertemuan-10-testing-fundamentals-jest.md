# Pertemuan 10: Testing Fundamentals — Jest & Vue Test Utils

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Menulis unit tests untuk functions dan components
- Menggunakan Jest untuk test runner
- Menggunakan Vue Test Utils untuk component testing
- Melakukan mocking untuk isolasi units
- Menghitung code coverage
- Mengimplementasikan test-driven development (TDD)

## 2. Pengantar: Hook

Code without tests adalah brittle. Tokopedia engineers push millions of lines of code per year. Tanpa comprehensive tests, regressions akan everywhere.

Testing bukan "nice to have". Tests adalah:
- Documentation (how should component work?)
- Regression prevention (refactoring confidence)
- Debugging aid (pinpoint issues faster)
- Quality gate (don't merge untested code)

## 3. Konsep Utama

### 3.1 Test Types

```
Unit Tests: Individual functions/components
├─ Fastest
├─ Most coverage
└─ Isolated (mocked dependencies)

Integration Tests: Multiple units together
├─ Slower
├─ Tests real interactions
└─ Semi-integrated

E2E Tests: Full app behavior
├─ Slowest
├─ Most realistic
└─ Full browser automation
```

### 3.2 Test Structure (AAA Pattern)

```javascript
describe('Counter', () => {
    it('should increment count', () => {
        // ARRANGE - setup
        const counter = new Counter();
        
        // ACT - do something
        counter.increment();
        
        // ASSERT - verify
        expect(counter.value).toBe(1);
    });
});
```

## 4. Ilustrasi & Analogi

**Analogi: Quality Control in Factory**

- **Unit test:** Test individual parts (screws, bolts)
- **Integration test:** Assemble parts, test subassembly
- **E2E test:** Full product test (does it work end-to-end?)

## 5. Contoh Teknis

### 5.1 Basic Jest Setup

```javascript
// jest.config.js
module.exports = {
    preset: '@vue/cli-plugin-unit-jest/presets/typescript-and-babel',
    transform: {
        '^.+\\.vue$': '@vue/vue3-jest',
        '.+\\.(css|styl|less|sass|scss|svg|png|jpg|ttf|woff|woff2)$': 
            'jest-transform-stub',
        '^.+\\.jsx?$': 'babel-jest'
    },
    testMatch: [
        '**/tests/unit/**/*.spec.(js|jsx|ts|tsx)',
        '**/__tests__/*.(js|jsx|ts|tsx)'
    ],
    collectCoverage: true,
    collectCoverageFrom: [
        'src/**/*.{js,vue}',
        '!src/main.js',
        '!**/node_modules/**'
    ],
    coveragePathIgnorePatterns: ['/node_modules/'],
    coverageThreshold: {
        global: { branches: 80, functions: 80, lines: 80, statements: 80 }
    }
};

// package.json
{
    "scripts": {
        "test": "jest",
        "test:watch": "jest --watch",
        "test:coverage": "jest --coverage"
    }
}
```

### 5.2 Unit Tests for Functions

```javascript
// src/utils/calculator.js
export const add = (a, b) => a + b;
export const multiply = (a, b) => a * b;
export const formatCurrency = (amount) => {
    return `Rp${amount.toLocaleString('id-ID')}`;
};

// tests/unit/calculator.spec.js
import { add, multiply, formatCurrency } from '@/utils/calculator';

describe('Calculator', () => {
    describe('add', () => {
        it('should add two numbers', () => {
            expect(add(2, 3)).toBe(5);
        });
        
        it('should handle negative numbers', () => {
            expect(add(-2, -3)).toBe(-5);
            expect(add(-2, 3)).toBe(1);
        });
        
        it('should handle zero', () => {
            expect(add(0, 5)).toBe(5);
        });
    });
    
    describe('multiply', () => {
        it('should multiply two numbers', () => {
            expect(multiply(2, 3)).toBe(6);
        });
        
        it('should handle zero', () => {
            expect(multiply(5, 0)).toBe(0);
        });
    });
    
    describe('formatCurrency', () => {
        it('should format amount with Rp prefix', () => {
            expect(formatCurrency(1000000)).toBe('Rp1.000.000');
        });
        
        it('should handle zero', () => {
            expect(formatCurrency(0)).toBe('Rp0');
        });
    });
});
```

### 5.3 Component Tests with Vue Test Utils

```javascript
// src/components/Counter.vue
<template>
    <div>
        <p>Count: {{ count }}</p>
        <button @click="increment">Increment</button>
        <button @click="decrement">Decrement</button>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    setup() {
        const count = ref(0);
        const increment = () => count.value++;
        const decrement = () => count.value--;
        return { count, increment, decrement };
    }
}
</script>

// tests/unit/Counter.spec.js
import { mount } from '@vue/test-utils';
import Counter from '@/components/Counter.vue';

describe('Counter.vue', () => {
    it('renders counter', () => {
        const wrapper = mount(Counter);
        expect(wrapper.text()).toContain('Count: 0');
    });
    
    it('increments count when button clicked', async () => {
        const wrapper = mount(Counter);
        await wrapper.find('button:first-of-type').trigger('click');
        expect(wrapper.text()).toContain('Count: 1');
    });
    
    it('decrements count when button clicked', async () => {
        const wrapper = mount(Counter);
        // Increment first
        await wrapper.findAll('button')[0].trigger('click');
        // Then decrement
        await wrapper.findAll('button')[1].trigger('click');
        expect(wrapper.text()).toContain('Count: 0');
    });
});
```

### 5.4 Testing Props & Emits

```javascript
// src/components/UserCard.vue
<template>
    <div class="user-card">
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
        <button @click="$emit('delete', user.id)">Delete</button>
    </div>
</template>

<script>
export default {
    props: {
        user: {
            type: Object,
            required: true
        }
    }
}
</script>

// tests/unit/UserCard.spec.js
import { mount } from '@vue/test-utils';
import UserCard from '@/components/UserCard.vue';

describe('UserCard.vue', () => {
    const user = { id: 1, name: 'Budi', email: 'budi@test.com' };
    
    it('renders user info', () => {
        const wrapper = mount(UserCard, { props: { user } });
        expect(wrapper.text()).toContain('Budi');
        expect(wrapper.text()).toContain('budi@test.com');
    });
    
    it('emits delete event when button clicked', async () => {
        const wrapper = mount(UserCard, { props: { user } });
        await wrapper.find('button').trigger('click');
        
        expect(wrapper.emitted('delete')).toBeTruthy();
        expect(wrapper.emitted('delete')[0]).toEqual([1]);
    });
});
```

### 5.5 Mocking & API Testing

```javascript
// src/services/userService.js
import axios from 'axios';

export const getUserById = async (id) => {
    const response = await axios.get(`/api/users/${id}`);
    return response.data;
};

// tests/unit/userService.spec.js
import { getUserById } from '@/services/userService';
import axios from 'axios';

jest.mock('axios');

describe('UserService', () => {
    afterEach(() => {
        jest.clearAllMocks();
    });
    
    it('should fetch user by id', async () => {
        const mockUser = { id: 1, name: 'Budi', email: 'budi@test.com' };
        
        // Mock axios response
        axios.get.mockResolvedValue({ data: mockUser });
        
        const result = await getUserById(1);
        
        expect(axios.get).toHaveBeenCalledWith('/api/users/1');
        expect(result).toEqual(mockUser);
    });
    
    it('should handle errors', async () => {
        axios.get.mockRejectedValue(new Error('Network error'));
        
        await expect(getUserById(1)).rejects.toThrow('Network error');
    });
});
```

### 5.6 Testing with Store (Pinia)

```javascript
// src/stores/counterStore.js
import { defineStore } from 'pinia';
import { ref } from 'vue';

export const useCounterStore = defineStore('counter', () => {
    const count = ref(0);
    const increment = () => count.value++;
    return { count, increment };
});

// tests/unit/counterStore.spec.js
import { setActivePinia, createPinia } from 'pinia';
import { useCounterStore } from '@/stores/counterStore';

describe('Counter Store', () => {
    beforeEach(() => {
        setActivePinia(createPinia());
    });
    
    it('increments count', () => {
        const store = useCounterStore();
        expect(store.count).toBe(0);
        
        store.increment();
        expect(store.count).toBe(1);
    });
});
```

## 6. Studi Kasus Nyata: Payment Button Component Test

```javascript
// src/components/PaymentButton.vue
<template>
    <button
        @click="handlePay"
        :disabled="isProcessing || amount <= 0"
        class="payment-btn"
    >
        {{ isProcessing ? 'Processing...' : `Pay Rp${amount}` }}
    </button>
</template>

<script>
import { ref } from 'vue';
import { paymentService } from '@/api/services/paymentService';

export default {
    props: {
        amount: {
            type: Number,
            required: true
        }
    },
    emits: ['payment-success', 'payment-error'],
    setup(props, { emit }) {
        const isProcessing = ref(false);
        
        const handlePay = async () => {
            isProcessing.value = true;
            try {
                const result = await paymentService.processPayment(props.amount);
                emit('payment-success', result);
            } catch (err) {
                emit('payment-error', err);
            } finally {
                isProcessing.value = false;
            }
        };
        
        return { isProcessing, handlePay };
    }
}
</script>

// tests/unit/PaymentButton.spec.js
import { mount } from '@vue/test-utils';
import PaymentButton from '@/components/PaymentButton.vue';
import { paymentService } from '@/api/services/paymentService';

jest.mock('@/api/services/paymentService');

describe('PaymentButton.vue', () => {
    it('renders button with amount', () => {
        const wrapper = mount(PaymentButton, { props: { amount: 100000 } });
        expect(wrapper.text()).toContain('Pay Rp100000');
    });
    
    it('disables button when amount is 0', () => {
        const wrapper = mount(PaymentButton, { props: { amount: 0 } });
        expect(wrapper.find('button').attributes('disabled')).toBeDefined();
    });
    
    it('calls payment service on click', async () => {
        paymentService.processPayment.mockResolvedValue({ id: 'TX-123' });
        
        const wrapper = mount(PaymentButton, { props: { amount: 100000 } });
        await wrapper.find('button').trigger('click');
        
        expect(paymentService.processPayment).toHaveBeenCalledWith(100000);
    });
    
    it('emits payment-success on successful payment', async () => {
        const mockResult = { id: 'TX-123', status: 'success' };
        paymentService.processPayment.mockResolvedValue(mockResult);
        
        const wrapper = mount(PaymentButton, { props: { amount: 100000 } });
        await wrapper.find('button').trigger('click');
        
        expect(wrapper.emitted('payment-success')[0]).toEqual([mockResult]);
    });
    
    it('shows processing state', async () => {
        paymentService.processPayment.mockImplementation(
            () => new Promise(r => setTimeout(r, 100))
        );
        
        const wrapper = mount(PaymentButton, { props: { amount: 100000 } });
        const button = wrapper.find('button');
        
        button.trigger('click');
        await wrapper.vm.$nextTick();
        
        expect(button.text()).toContain('Processing...');
    });
});
```

## 7. Visualisasi: Test Coverage Report

```
$ npm run test:coverage

────────────────────────────────────────────
File               | % Stmts | % Branch | % Funcs
────────────────────────────────────────────
All files          |  85.2   |  82.1    |  88.9
  calculator.js    |  100    |  100     |  100
  utils.js         |  75     |  60      |  80
  Counter.vue      |  92     |  85      |  95
────────────────────────────────────────────
```

## 8. Kesalahan Umum

### ❌ Testing implementation, not behavior

```javascript
// ❌ WRONG - tests internal state
it('sets count to 1', () => {
    const counter = new Counter();
    counter.increment();
    expect(counter.count).toBe(1); // testing internal
});

// ✅ CORRECT - tests behavior
it('displays 1 after increment', () => {
    const wrapper = mount(Counter);
    wrapper.find('button').trigger('click');
    expect(wrapper.text()).toContain('Count: 1');
});
```

### ❌ Not mocking external dependencies

```javascript
// ❌ WRONG - makes real API calls
it('fetches user', async () => {
    const user = await userService.getUser(1); // real request!
});

// ✅ CORRECT - mocks API
jest.mock('axios');
axios.get.mockResolvedValue({ data: mockUser });
const user = await userService.getUser(1);
```

## 9. Latihan & Studi Kasus

### Latihan 1: Unit Test Function
```javascript
// Task: Write tests for isValidEmail function
// - Valid: test@example.com
// - Invalid: missing @, domain
```

### Latihan 2: Component Test
```javascript
// Task: Write tests for Button component
// - Renders with label
// - Calls onClick when clicked
// - Disables when isDisabled is true
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami test types (unit, integration, e2e)
- [ ] Bisa write unit tests dengan Jest
- [ ] Bisa write component tests dengan Vue Test Utils
- [ ] Bisa mock functions dan API calls
- [ ] Bisa test component props dan emits
- [ ] Bisa test with Pinia stores
- [ ] Mengerti AAA pattern (Arrange, Act, Assert)
- [ ] Bisa measure code coverage
- [ ] Mengerti test-driven development
- [ ] Tahu apa yang sebaiknya ditest

## 11. Referensi

- [Jest Documentation](https://jestjs.io)
- [Vue Test Utils](https://test-utils.vuejs.org)
- [Testing Library](https://testing-library.com)

**Status:** ✅ Pertemuan 10 selesai
