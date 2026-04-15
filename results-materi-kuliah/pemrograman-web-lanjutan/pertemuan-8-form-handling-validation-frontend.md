# Pertemuan 8: Form Handling & Validation — Frontend

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Mengimplementasikan client-side form validation
- Menggunakan Vee-Validate untuk complex validations
- Membuat custom validators untuk business logic
- Menangani async validations (check email availability)
- Menampilkan error messages secara user-friendly
- Mengimplementasikan form state management
- Membuat reusable form components

## 2. Pengantar: Hook

Form adalah interface antara user dan aplikasi. Validasi yang buruk = user confusion = bad UX. DANA KYC form harus validate:
- Format email
- Phone number country code
- ID card validity
- Document upload

Tapi server tidak boleh trust client. Client validation adalah untuk UX. Server validation adalah untuk security.

## 3. Konsep Utama

### 3.1 Validation Types

```
Client-side (UX): Real-time feedback, fast, no server call
Server-side (Security): Validate everything, never trust client
Async Validation: Check email availability, username uniqueness
```

### 3.2 Vee-Validate Architecture

```javascript
// Define validation schema once
const schema = {
    email: 'required|email|unique:users',
    password: 'required|min:8|strongPassword',
    phone: 'required|phone:ID'
};

// Use in form
// Validation runs automatically
// Errors displayed automatically
// Submit only if valid
```

## 4. Ilustrasi & Analogi

**Analogi: Bank Form**

- **Required validation:** Bank must have name field
- **Format validation:** Phone must be 10+ digits
- **Async validation:** Account number must exist in system
- **Custom validation:** Must be 18+ years old
- **Cross-field:** Password and confirm must match

## 5. Contoh Teknis

### 5.1 Basic Form with HTML5 Validation

```javascript
// src/components/SimpleForm.vue
<template>
    <form @submit.prevent="handleSubmit" class="form">
        <div class="form-group">
            <label>Name:</label>
            <input 
                v-model="form.name"
                type="text"
                required
                minlength="2"
                @blur="validateName"
            />
            <span v-if="errors.name" class="error">{{ errors.name }}</span>
        </div>
        
        <div class="form-group">
            <label>Email:</label>
            <input 
                v-model="form.email"
                type="email"
                required
                @blur="validateEmail"
            />
            <span v-if="errors.email" class="error">{{ errors.email }}</span>
        </div>
        
        <div class="form-group">
            <label>Phone:</label>
            <input 
                v-model="form.phone"
                type="tel"
                pattern="08[0-9]{9,11}"
                @blur="validatePhone"
            />
            <span v-if="errors.phone" class="error">{{ errors.phone }}</span>
        </div>
        
        <button type="submit" :disabled="!isFormValid">
            Submit
        </button>
    </form>
</template>

<script>
import { reactive, ref, computed } from 'vue';

export default {
    setup() {
        const form = reactive({
            name: '',
            email: '',
            phone: ''
        });
        
        const errors = reactive({});
        
        const validateName = () => {
            errors.name = '';
            if (!form.name) {
                errors.name = 'Name is required';
            } else if (form.name.length < 2) {
                errors.name = 'Min 2 characters';
            }
        };
        
        const validateEmail = () => {
            errors.email = '';
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!form.email) {
                errors.email = 'Email is required';
            } else if (!emailRegex.test(form.email)) {
                errors.email = 'Invalid email format';
            }
        };
        
        const validatePhone = () => {
            errors.phone = '';
            const phoneRegex = /^08[0-9]{9,11}$/;
            if (!form.phone) {
                errors.phone = 'Phone is required';
            } else if (!phoneRegex.test(form.phone)) {
                errors.phone = 'Phone format: 08XXXXXXXXX';
            }
        };
        
        const isFormValid = computed(() =>
            form.name && form.email && form.phone &&
            !errors.name && !errors.email && !errors.phone
        );
        
        const handleSubmit = async () => {
            validateName();
            validateEmail();
            validatePhone();
            
            if (!isFormValid.value) return;
            
            // Submit form
            const response = await fetch('/api/register', {
                method: 'POST',
                body: JSON.stringify(form)
            });
            
            if (response.ok) {
                console.log('Success!');
            }
        };
        
        return { form, errors, isFormValid, handleSubmit };
    }
}
</script>

<style scoped>
.form { max-width: 400px; }
.form-group { margin-bottom: 20px; }
label { display: block; margin-bottom: 5px; font-weight: bold; }
input { width: 100%; padding: 8px; border: 1px solid #ddd; }
.error { color: red; font-size: 0.9em; }
button:disabled { opacity: 0.5; cursor: not-allowed; }
</style>
```

### 5.2 Vee-Validate with Schema

```javascript
// src/components/KYCForm.vue
<template>
    <form @submit.prevent="handleSubmit" class="kyc-form">
        <!-- Full Name -->
        <div class="form-group">
            <label>Full Name:</label>
            <input v-model="form.fullName" />
            <span v-if="errors.fullName" class="error">
                {{ errors.fullName }}
            </span>
        </div>
        
        <!-- Email -->
        <div class="form-group">
            <label>Email:</label>
            <input 
                v-model="form.email"
                @blur="checkEmailUnique"
            />
            <span v-if="errors.email" class="error">
                {{ errors.email }}
            </span>
            <span v-if="emailChecking" class="checking">Checking...</span>
        </div>
        
        <!-- Password -->
        <div class="form-group">
            <label>Password:</label>
            <input 
                v-model="form.password"
                type="password"
                @input="validatePassword"
            />
            <span v-if="errors.password" class="error">
                {{ errors.password }}
            </span>
            <div class="strength" :class="passwordStrength">
                {{ passwordStrengthText }}
            </div>
        </div>
        
        <!-- Confirm Password -->
        <div class="form-group">
            <label>Confirm Password:</label>
            <input 
                v-model="form.confirmPassword"
                type="password"
            />
            <span v-if="errors.confirmPassword" class="error">
                {{ errors.confirmPassword }}
            </span>
        </div>
        
        <!-- ID Card -->
        <div class="form-group">
            <label>ID Card Number:</label>
            <input v-model="form.idCard" />
            <span v-if="errors.idCard" class="error">
                {{ errors.idCard }}
            </span>
        </div>
        
        <!-- Phone -->
        <div class="form-group">
            <label>Phone:</label>
            <input v-model="form.phone" />
            <span v-if="errors.phone" class="error">
                {{ errors.phone }}
            </span>
        </div>
        
        <button type="submit" :disabled="!isFormValid || isSubmitting">
            {{ isSubmitting ? 'Submitting...' : 'Register' }}
        </button>
    </form>
</template>

<script>
import { reactive, ref, computed } from 'vue';

export default {
    setup() {
        const form = reactive({
            fullName: '',
            email: '',
            password: '',
            confirmPassword: '',
            idCard: '',
            phone: ''
        });
        
        const errors = reactive({});
        const emailChecking = ref(false);
        const isSubmitting = ref(false);
        
        // Validation rules
        const validateFullName = () => {
            if (!form.fullName) {
                errors.fullName = 'Name required';
            } else if (form.fullName.length < 3) {
                errors.fullName = 'Min 3 characters';
            } else {
                errors.fullName = '';
            }
        };
        
        const validatePassword = () => {
            const pwd = form.password;
            
            if (!pwd) {
                errors.password = 'Password required';
            } else if (pwd.length < 8) {
                errors.password = 'Min 8 characters';
            } else if (!/[A-Z]/.test(pwd)) {
                errors.password = 'Need uppercase letter';
            } else if (!/[0-9]/.test(pwd)) {
                errors.password = 'Need digit';
            } else {
                errors.password = '';
            }
        };
        
        const validateConfirmPassword = () => {
            if (form.confirmPassword !== form.password) {
                errors.confirmPassword = 'Passwords do not match';
            } else {
                errors.confirmPassword = '';
            }
        };
        
        const validatePhone = () => {
            const phoneRegex = /^08[0-9]{9,11}$/;
            if (!form.phone) {
                errors.phone = 'Phone required';
            } else if (!phoneRegex.test(form.phone)) {
                errors.phone = 'Invalid format';
            } else {
                errors.phone = '';
            }
        };
        
        const validateIdCard = () => {
            if (!form.idCard) {
                errors.idCard = 'ID card required';
            } else if (form.idCard.length !== 16) {
                errors.idCard = 'Must be 16 digits';
            } else {
                errors.idCard = '';
            }
        };
        
        // Async validation
        const checkEmailUnique = async () => {
            if (!form.email) {
                errors.email = 'Email required';
                return;
            }
            
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailRegex.test(form.email)) {
                errors.email = 'Invalid email format';
                return;
            }
            
            emailChecking.value = true;
            try {
                const response = await fetch(
                    `/api/check-email?email=${form.email}`
                );
                const data = await response.json();
                
                if (!data.available) {
                    errors.email = 'Email already registered';
                } else {
                    errors.email = '';
                }
            } finally {
                emailChecking.value = false;
            }
        };
        
        // Password strength indicator
        const passwordStrength = computed(() => {
            const pwd = form.password;
            let strength = 0;
            
            if (pwd.length >= 8) strength++;
            if (pwd.length >= 12) strength++;
            if (/[A-Z]/.test(pwd) && /[a-z]/.test(pwd)) strength++;
            if (/[0-9]/.test(pwd)) strength++;
            if (/[^A-Za-z0-9]/.test(pwd)) strength++;
            
            if (strength <= 2) return 'weak';
            if (strength <= 3) return 'medium';
            return 'strong';
        });
        
        const passwordStrengthText = computed(() => {
            const map = { weak: 'Weak', medium: 'Medium', strong: 'Strong' };
            return map[passwordStrength.value];
        });
        
        const isFormValid = computed(() => {
            return form.fullName && form.email && form.password &&
                   form.confirmPassword && form.idCard && form.phone &&
                   !Object.values(errors).some(e => e);
        });
        
        const handleSubmit = async () => {
            // Validate all fields
            validateFullName();
            await checkEmailUnique();
            validatePassword();
            validateConfirmPassword();
            validatePhone();
            validateIdCard();
            
            if (!isFormValid.value) return;
            
            isSubmitting.value = true;
            try {
                const response = await fetch('/api/register', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(form)
                });
                
                if (response.ok) {
                    console.log('Registration successful!');
                    // Redirect to success page
                }
            } finally {
                isSubmitting.value = false;
            }
        };
        
        return {
            form,
            errors,
            emailChecking,
            isSubmitting,
            passwordStrength,
            passwordStrengthText,
            isFormValid,
            handleSubmit,
            validatePassword
        };
    }
}
</script>

<style scoped>
.kyc-form { max-width: 500px; }
.form-group { margin-bottom: 20px; }
label { display: block; font-weight: bold; margin-bottom: 5px; }
input { width: 100%; padding: 10px; border: 1px solid #ddd; }
.error { color: #d32f2f; font-size: 0.85em; }
.checking { color: #f57c00; font-size: 0.85em; }
.strength { padding: 5px; margin-top: 5px; border-radius: 3px; }
.weak { background: #ffebee; color: #d32f2f; }
.medium { background: #fff3e0; color: #f57c00; }
.strong { background: #e8f5e9; color: #388e3c; }
</style>
```

### 5.3 Custom Validators

```javascript
// src/validators/customValidators.js
export const validators = {
    // Phone Indonesia format
    phoneIndonesia: (value) => {
        if (!value) return false;
        return /^08[0-9]{9,11}$/.test(value);
    },
    
    // Strong password
    strongPassword: (value) => {
        return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d@$!%*?&]{8,}$/.test(value);
    },
    
    // Valid ID card (KTP)
    validIdCard: (value) => {
        // Check 16 digits
        if (!value || value.length !== 16) return false;
        // Could add Luhn algorithm check here
        return /^\d{16}$/.test(value);
    },
    
    // Credit card
    validCreditCard: (value) => {
        // Luhn algorithm
        if (!value) return false;
        const digits = value.replace(/\D/g, '');
        let sum = 0;
        for (let i = 0; i < digits.length; i++) {
            let digit = parseInt(digits[i]);
            if ((digits.length - i - 1) % 2 === 1) {
                digit *= 2;
                if (digit > 9) digit -= 9;
            }
            sum += digit;
        }
        return sum % 10 === 0;
    },
    
    // Age check
    minAge: (minYears) => (value) => {
        const birthDate = new Date(value);
        const today = new Date();
        const age = today.getFullYear() - birthDate.getFullYear();
        const monthDiff = today.getMonth() - birthDate.getMonth();
        if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birthDate.getDate())) {
            return age - 1 >= minYears;
        }
        return age >= minYears;
    }
};

// Usage in component
const validateAge = (birthDate) => {
    if (!validators.minAge(18)(birthDate)) {
        errors.birthDate = 'Must be 18+ years old';
    }
};
```

## 6. Studi Kasus Nyata: DANA KYC Form

DANA KYC (Know Your Customer) form requires:
- Personal info validation
- ID card verification
- Phone verification (OTP)
- Address validation
- Document upload

```javascript
// src/components/KYCStep1PersonalInfo.vue
// Step 1: Basic info + async email check
// Step 2: ID card upload + verification
// Step 3: Phone verification with OTP
// Step 4: Address + confirmation

// This multi-step form uses state store to persist data
// and track completion progress
```

## 7. Visualisasi: Form Validation Flow

```
User inputs data
    ↓
On change/blur
    ↓
Validate synchronously
    ↓ Valid? 
    ├─ Yes → Clear error
    └─ No → Show error
    ↓
Async validation (if needed)
    ↓
Check email unique/username available
    ↓
User clicks submit
    ↓
Validate all fields
    ↓
All valid?
    ├─ Yes → Submit
    └─ No → Show errors
```

## 8. Kesalahan Umum

### ❌ Only validating on submit

```javascript
// ❌ WRONG - user not informed until submit
<input v-model="email" />
<button @click="submit">Submit</button>

// ✅ CORRECT - validate on blur for immediate feedback
<input 
    v-model="email"
    @blur="validateEmail"
/>
<span v-if="errors.email">{{ errors.email }}</span>
```

### ❌ Trusting client validation only

```javascript
// ❌ WRONG - server doesn't validate
app.post('/register', (req, res) => {
    const { email, password } = req.body;
    // Save directly without validation!
    User.create({ email, password });
});

// ✅ CORRECT - server validates everything
app.post('/register', (req, res) => {
    const { error } = registrationSchema.validate(req.body);
    if (error) return res.status(400).json({ error });
    // Process...
});
```

## 9. Latihan & Studi Kasus

### Latihan 1: Simple Form Validation
```javascript
// Task: Create form with
// - Username (3+ chars)
// - Email (valid format)
// - Password (8+ chars, uppercase, digit)
// - Show real-time errors
```

### Latihan 2: Async Email Check
```javascript
// Task: Check email availability on blur
// - Call /api/check-email?email=x@test.com
// - Show "Checking..." while fetching
// - Show error if not available
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami client-side vs server-side validation
- [ ] Bisa implement form validation manually
- [ ] Bisa handle async validations
- [ ] Bisa show/hide error messages
- [ ] Bisa disable submit button when invalid
- [ ] Bisa create custom validators
- [ ] Bisa implement password strength indicator
- [ ] Mengerti form UX best practices
- [ ] Bisa handle form state
- [ ] Tahu ketika pakai Vee-Validate vs manual

## 11. Referensi

- [HTML5 Validation](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)
- [Vee-Validate Documentation](https://vee-validate.logaretm.com/)
- [RegEx for form validation](https://regexr.com/)
- [Form UX Best Practices](https://www.smashingmagazine.com/articles/inline-validation-web-forms-real-time-experience/)

**Status:** ✅ Pertemuan 8 selesai
