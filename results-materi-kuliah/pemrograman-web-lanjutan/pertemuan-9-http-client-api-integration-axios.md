# Pertemuan 9: HTTP Client & API Integration — Axios

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Menggunakan Axios untuk HTTP requests (GET, POST, PUT, DELETE)
- Mengimplementasikan request/response interceptors
- Menangani error handling dalam API calls
- Mengimplementasikan retry logic dan timeout
- Membuat API client library yang reusable
- Menangani authentication tokens dalam requests
- Mengintegrasikan dengan backend APIs

## 2. Pengantar: Hook

Frontend berkomunikasi dengan backend melalui HTTP. Axios adalah library populer karena:
- Request/response interceptors (add auth token automatically)
- Timeout handling (cancel long requests)
- Retry logic (network failures)
- File upload support

Xendit payment gateway require precise error handling. Kalau timeout tidak ditangani, payment hangs. Kalau 401 tidak refresh token, user kena logout random.

## 3. Konsep Utama

### 3.1 HTTP Methods

```
GET    → Fetch data (safe, idempotent)
POST   → Create resource (not safe, not idempotent)
PUT    → Replace resource (idempotent)
PATCH  → Partial update (may not be idempotent)
DELETE → Remove resource (idempotent)
```

### 3.2 Interceptor Pipeline

```
Request:
1. Add auth token to header
2. Add request ID
3. Log request
4. Send to server

Response:
1. Check status code
2. Parse JSON
3. Log response
4. Return data to component

Error:
1. Check if 401 (expired token)
2. Refresh token
3. Retry request
4. Or show error
```

## 4. Ilustrasi & Analogi

**Analogi: Post Office**

- **Request:** Letter with address
- **Interceptor:** Post office adds tracking number
- **Server:** Sorting facility
- **Response:** Reply letter
- **Error:** Return to sender if address wrong
- **Retry:** Try different route if first failed

## 5. Contoh Teknis

### 5.1 Basic Axios Setup

```javascript
// src/api/client.js
import axios from 'axios';

const apiClient = axios.create({
    baseURL: process.env.VUE_APP_API_BASE_URL || 'http://localhost:3000/api',
    timeout: 10000, // 10 second timeout
    headers: {
        'Content-Type': 'application/json'
    }
});

export default apiClient;

// src/api/services/userService.js
import apiClient from '../client';

export const userService = {
    // GET user profile
    getProfile: async (userId) => {
        const response = await apiClient.get(`/users/${userId}`);
        return response.data;
    },
    
    // POST create user
    createUser: async (userData) => {
        const response = await apiClient.post('/users', userData);
        return response.data;
    },
    
    // PUT update user
    updateUser: async (userId, userData) => {
        const response = await apiClient.put(`/users/${userId}`, userData);
        return response.data;
    },
    
    // DELETE remove user
    deleteUser: async (userId) => {
        const response = await apiClient.delete(`/users/${userId}`);
        return response.data;
    }
};

// Component usage
<script>
import { userService } from '@/api/services/userService';

export default {
    async setup() {
        const user = await userService.getProfile(123);
        return { user };
    }
}
</script>
```

### 5.2 Request/Response Interceptors

```javascript
// src/api/client.js
import axios from 'axios';

const apiClient = axios.create({
    baseURL: 'http://localhost:3000/api',
    timeout: 10000
});

// REQUEST INTERCEPTOR
apiClient.interceptors.request.use(
    (config) => {
        // 1. Add auth token
        const token = localStorage.getItem('accessToken');
        if (token) {
            config.headers.Authorization = `Bearer ${token}`;
        }
        
        // 2. Add request ID for tracking
        config.headers['X-Request-ID'] = generateRequestId();
        
        // 3. Log request
        console.log(`[Request] ${config.method.toUpperCase()} ${config.url}`);
        
        return config;
    },
    (error) => {
        return Promise.reject(error);
    }
);

// RESPONSE INTERCEPTOR
apiClient.interceptors.response.use(
    (response) => {
        // Success: just return data
        console.log(`[Response] ${response.status} ${response.config.url}`);
        return response;
    },
    async (error) => {
        const originalRequest = error.config;
        
        // Handle 401: Token expired
        if (error.response?.status === 401 && !originalRequest._retry) {
            originalRequest._retry = true;
            
            try {
                // Try to refresh token
                const refreshToken = localStorage.getItem('refreshToken');
                const response = await axios.post(
                    'http://localhost:3000/api/auth/refresh',
                    { refreshToken }
                );
                
                const { accessToken } = response.data;
                localStorage.setItem('accessToken', accessToken);
                
                // Retry original request with new token
                originalRequest.headers.Authorization = `Bearer ${accessToken}`;
                return apiClient(originalRequest);
            } catch (refreshError) {
                // Refresh failed: redirect to login
                window.location.href = '/login';
                return Promise.reject(refreshError);
            }
        }
        
        // Handle 403: Permission denied
        if (error.response?.status === 403) {
            console.error('Permission denied:', error.response.data.message);
            // Redirect to unauthorized page
            window.location.href = '/unauthorized';
        }
        
        // Handle 5xx: Server error
        if (error.response?.status >= 500) {
            console.error('Server error:', error.response.status);
            // Show error toast/notification
        }
        
        // Network error
        if (!error.response) {
            console.error('Network error:', error.message);
        }
        
        return Promise.reject(error);
    }
);

function generateRequestId() {
    return `${Date.now()}-${Math.random().toString(36).substr(2, 9)}`;
}

export default apiClient;
```

### 5.3 Error Handling

```javascript
// src/api/handlers/errorHandler.js
export class ApiError extends Error {
    constructor(message, status, data) {
        super(message);
        this.status = status;
        this.data = data;
    }
}

// src/composables/useApi.js
import { ref } from 'vue';
import apiClient from '@/api/client';
import { ApiError } from '@/api/handlers/errorHandler';

export const useApi = () => {
    const isLoading = ref(false);
    const error = ref(null);
    
    const request = async (method, url, data = null) => {
        isLoading.value = true;
        error.value = null;
        
        try {
            let response;
            
            if (method === 'get') {
                response = await apiClient.get(url);
            } else if (method === 'post') {
                response = await apiClient.post(url, data);
            } else if (method === 'put') {
                response = await apiClient.put(url, data);
            } else if (method === 'delete') {
                response = await apiClient.delete(url);
            }
            
            return response.data;
        } catch (err) {
            const message = err.response?.data?.message || 
                          err.message || 
                          'Something went wrong';
            
            error.value = new ApiError(
                message,
                err.response?.status,
                err.response?.data
            );
            
            throw error.value;
        } finally {
            isLoading.value = false;
        }
    };
    
    return {
        isLoading,
        error,
        request,
        get: (url) => request('get', url),
        post: (url, data) => request('post', url, data),
        put: (url, data) => request('put', url, data),
        delete: (url) => request('delete', url)
    };
};

// Component usage
<script>
import { useApi } from '@/composables/useApi';

export default {
    setup() {
        const { get, isLoading, error } = useApi();
        
        const loadUser = async (id) => {
            try {
                return await get(`/users/${id}`);
            } catch (err) {
                console.error('Failed to load user:', err.message);
            }
        };
        
        return { loadUser, isLoading, error };
    }
}
</script>
```

### 5.4 Retry Logic with Exponential Backoff

```javascript
// src/api/utils/retryRequest.js
export const retryRequest = async (
    requestFn,
    maxRetries = 3,
    initialDelay = 1000
) => {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            return await requestFn();
        } catch (error) {
            // Don't retry on client errors (4xx)
            if (error.response?.status >= 400 && 
                error.response?.status < 500) {
                throw error;
            }
            
            // Don't retry on last attempt
            if (attempt === maxRetries) {
                throw error;
            }
            
            // Exponential backoff
            const delay = initialDelay * Math.pow(2, attempt - 1);
            console.log(`Retry attempt ${attempt} in ${delay}ms...`);
            
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
};

// src/composables/useRetryApi.js
export const useRetryApi = () => {
    const { request } = useApi();
    
    const requestWithRetry = async (method, url, data = null) => {
        return retryRequest(() => request(method, url, data));
    };
    
    return { requestWithRetry };
};
```

### 5.5 File Upload

```javascript
// src/composables/useFileUpload.js
import apiClient from '@/api/client';

export const useFileUpload = () => {
    const uploadFile = async (file, endpoint = '/upload') => {
        const formData = new FormData();
        formData.append('file', file);
        
        return apiClient.post(endpoint, formData, {
            headers: {
                'Content-Type': 'multipart/form-data'
            },
            onUploadProgress: (progressEvent) => {
                const percentCompleted = Math.round(
                    (progressEvent.loaded * 100) / progressEvent.total
                );
                return percentCompleted;
            }
        });
    };
    
    return { uploadFile };
};

// Component usage
<template>
    <input type="file" @change="handleFileSelect" />
    <div v-if="uploadProgress" class="progress">
        {{ uploadProgress }}%
    </div>
</template>

<script>
import { ref } from 'vue';
import { useFileUpload } from '@/composables/useFileUpload';

export default {
    setup() {
        const { uploadFile } = useFileUpload();
        const uploadProgress = ref(0);
        
        const handleFileSelect = async (event) => {
            const file = event.target.files[0];
            if (!file) return;
            
            try {
                await uploadFile(file);
            } catch (err) {
                console.error('Upload failed:', err);
            }
        };
        
        return { uploadProgress, handleFileSelect };
    }
}
</script>
```

## 6. Studi Kasus Nyata: Xendit Payment Integration

Xendit is payment gateway. Integration requires:
- Create invoice (POST)
- Check payment status (GET with polling)
- Handle webhooks
- Retry on network failure

```javascript
// src/api/services/paymentService.js
import apiClient from '../client';
import { retryRequest } from '../utils/retryRequest';

export const paymentService = {
    // Create invoice at Xendit
    createInvoice: async (amount, description, customerId) => {
        return retryRequest(async () => {
            const response = await apiClient.post('/invoices', {
                amount,
                description,
                customerId,
                externalId: `ORDER-${Date.now()}`
            });
            return response.data;
        });
    },
    
    // Poll for payment status
    checkPaymentStatus: async (invoiceId) => {
        return retryRequest(async () => {
            const response = await apiClient.get(`/invoices/${invoiceId}`);
            return response.data;
        });
    },
    
    // Poll until paid with timeout
    waitForPayment: async (invoiceId, maxWaitMs = 300000) => {
        const startTime = Date.now();
        const pollInterval = 2000; // 2 seconds
        
        while (Date.now() - startTime < maxWaitMs) {
            try {
                const invoice = await this.checkPaymentStatus(invoiceId);
                
                if (invoice.status === 'PAID') {
                    return invoice;
                }
                
                if (invoice.status === 'EXPIRED') {
                    throw new Error('Invoice expired');
                }
                
                // Wait before polling again
                await new Promise(r => setTimeout(r, pollInterval));
            } catch (err) {
                throw err;
            }
        }
        
        throw new Error('Payment timeout');
    }
};

// Component usage
<template>
    <div v-if="!orderId" class="checkout">
        <button @click="createOrder">Pay Now</button>
    </div>
    <div v-else class="waiting">
        <p>Waiting for payment...</p>
        <div v-if="isPaying" class="spinner"></div>
    </div>
</template>

<script>
import { ref } from 'vue';
import { paymentService } from '@/api/services/paymentService';

export default {
    setup() {
        const orderId = ref(null);
        const isPaying = ref(false);
        
        const createOrder = async () => {
            try {
                const invoice = await paymentService.createInvoice(
                    100000,
                    'Order payment',
                    'CUST-123'
                );
                
                orderId.value = invoice.id;
                isPaying.value = true;
                
                // Wait for payment
                const paidInvoice = await paymentService.waitForPayment(
                    invoice.id
                );
                
                console.log('Payment received!', paidInvoice);
                // Redirect to success
            } catch (err) {
                console.error('Payment error:', err.message);
            } finally {
                isPaying.value = false;
            }
        };
        
        return { orderId, isPaying, createOrder };
    }
}
</script>
```

## 7. Visualisasi: Request/Response Cycle with Interceptors

```
Component calls:
    ↓
apiClient.post('/users', data)
    ↓
Request Interceptor:
├─ Add auth token
├─ Add request ID
├─ Log request
    ↓
Send to server
    ↓
Server processes
    ↓
Response comes back
    ↓
Response Interceptor:
├─ Check status (200, 401, 500?)
├─ Handle 401 → refresh token → retry
├─ Handle error → reject promise
├─ Log response
    ↓
Return data to component
```

## 8. Kesalahan Umum

### ❌ Not handling auth token refresh

```javascript
// ❌ WRONG - 401 just rejects
apiClient.interceptors.response.use(null, err => Promise.reject(err));

// ✅ CORRECT - 401 triggers refresh
if (error.response?.status === 401 && !originalRequest._retry) {
    originalRequest._retry = true;
    const newToken = await refreshToken();
    originalRequest.headers.Authorization = `Bearer ${newToken}`;
    return apiClient(originalRequest);
}
```

### ❌ Not handling timeout

```javascript
// ❌ WRONG - requests can hang forever
const client = axios.create();

// ✅ CORRECT - set timeout
const client = axios.create({ timeout: 10000 });
```

### ❌ No error handling

```javascript
// ❌ WRONG
const data = await apiClient.get('/data');

// ✅ CORRECT
try {
    const data = await apiClient.get('/data');
} catch (err) {
    console.error(err.message);
}
```

## 9. Latihan & Studi Kasus

### Latihan 1: Basic CRUD API
```javascript
// Task: Create API service for users
// - GET /users/:id
// - POST /users (create)
// - PUT /users/:id (update)
// - DELETE /users/:id
```

### Latihan 2: Error Handling
```javascript
// Task: Create interceptor that:
// - Catches 401 and refreshes token
// - Retries failed request
// - Shows error toast on other errors
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami HTTP methods (GET/POST/PUT/DELETE)
- [ ] Bisa setup Axios dengan baseURL
- [ ] Bisa implement request interceptor
- [ ] Bisa implement response interceptor
- [ ] Bisa handle 401 with token refresh
- [ ] Bisa implement retry logic
- [ ] Bisa handle file upload
- [ ] Bisa use async/await with promises
- [ ] Bisa create reusable API service
- [ ] Mengerti timeout dan error handling

## 11. Referensi

- [Axios Documentation](https://axios-http.com)
- [HTTP Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [REST API Best Practices](https://restfulapi.net)

**Status:** ✅ Pertemuan 9 selesai
