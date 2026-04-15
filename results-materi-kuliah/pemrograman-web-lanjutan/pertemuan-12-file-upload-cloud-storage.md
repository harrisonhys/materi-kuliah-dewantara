# Pertemuan 12: File Upload & Cloud Storage — Multer, AWS S3

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Mengimplementasikan file upload di backend dengan multer
- Melakukan validasi file (type, size, virus scan)
- Mengintegrasikan AWS S3 untuk cloud storage
- Mengimplementasikan upload progress tracking
- Menangani chunked/resumable uploads
- Membuat secure file download endpoints
- Mengoptimalkan file storage dan CDN delivery

## 2. Pengantar: Hook

Gojek driver upload KTP, license, SIM untuk verification. Files harus:
- Scanned untuk virus
- Validated (not corrupted)
- Stored securely (encrypted)
- Served fast (CDN)
- Deleted when expired

Local disk storage = not scalable. AWS S3 = industry standard. You pay per GB, but no infrastructure overhead.

## 3. Konsep Utama

### 3.1 Upload Flow

```
User selects file
    ↓
Browser validates (size, type)
    ↓
Send to server (multipart/form-data)
    ↓
Server validates again (security)
    ↓
Store to S3
    ↓
Return S3 URL to client
    ↓
Display in UI
```

### 3.2 File Security

```
Client-side: Fast validation (UX)
Server-side: Real validation (security)

Never trust client!
```

## 4. Ilustrasi & Analogi

**Analogi: Bank Document Upload**

- **Validation:** Check format (must be PDF)
- **Virus scan:** Scan for malware
- **Storage:** Vault (encrypted S3)
- **Delivery:** On-demand retrieval (CloudFront CDN)

## 5. Contoh Teknis

### 5.1 Backend File Upload with Multer

```javascript
// npm install multer aws-sdk

// src/middleware/upload.js
const multer = require('multer');
const path = require('path');

const storage = multer.memoryStorage(); // keep in memory before S3

const fileFilter = (req, file, cb) => {
    const allowedMimes = ['image/jpeg', 'image/png', 'application/pdf'];
    const allowedExts = ['.jpg', '.jpeg', '.png', '.pdf'];
    
    const ext = path.extname(file.originalname).toLowerCase();
    
    if (!allowedMimes.includes(file.mimetype)) {
        return cb(new Error('Only JPEG, PNG, PDF allowed'));
    }
    
    if (!allowedExts.includes(ext)) {
        return cb(new Error('Invalid file extension'));
    }
    
    if (file.size > 5 * 1024 * 1024) {
        return cb(new Error('File too large (max 5MB)'));
    }
    
    cb(null, true);
};

const upload = multer({
    storage,
    fileFilter,
    limits: { fileSize: 5 * 1024 * 1024 }
});

module.exports = upload;

// src/routes/upload.js
const express = require('express');
const AWS = require('aws-sdk');
const upload = require('../middleware/upload');

const router = express.Router();

const s3 = new AWS.S3({
    accessKeyId: process.env.AWS_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY
});

router.post('/upload', upload.single('file'), async (req, res, next) => {
    try {
        if (!req.file) {
            return res.status(400).json({ error: 'No file uploaded' });
        }
        
        // Scan for virus (optional, use ClamAV)
        // await virusScanner.scan(req.file.buffer);
        
        // Upload to S3
        const key = `uploads/${Date.now()}-${req.file.originalname}`;
        const params = {
            Bucket: process.env.AWS_S3_BUCKET,
            Key: key,
            Body: req.file.buffer,
            ContentType: req.file.mimetype,
            ACL: 'private' // not publicly readable
        };
        
        const result = await s3.upload(params).promise();
        
        // Save metadata to database
        await FileMetadata.create({
            userId: req.user.id,
            filename: req.file.originalname,
            s3Key: result.Key,
            size: req.file.size,
            mimetype: req.file.mimetype,
            url: result.Location
        });
        
        res.json({
            success: true,
            url: result.Location,
            key: result.Key
        });
    } catch (err) {
        next(err);
    }
});

// Download (with access control)
router.get('/download/:fileId', authenticateToken, async (req, res, next) => {
    try {
        const file = await FileMetadata.findByPk(req.params.fileId);
        
        if (!file || file.userId !== req.user.id) {
            return res.status(403).json({ error: 'Access denied' });
        }
        
        // Generate signed URL (expires in 1 hour)
        const signedUrl = s3.getSignedUrl('getObject', {
            Bucket: process.env.AWS_S3_BUCKET,
            Key: file.s3Key,
            Expires: 3600
        });
        
        res.json({ url: signedUrl });
    } catch (err) {
        next(err);
    }
});

module.exports = router;
```

### 5.2 Frontend File Upload with Progress

```javascript
// src/components/FileUpload.vue
<template>
    <div class="file-upload">
        <div 
            @drop="handleDrop"
            @dragover.prevent
            @dragleave.prevent
            class="drop-zone"
            :class="{ 'is-dragging': isDragging }"
        >
            <input 
                type="file"
                ref="fileInput"
                @change="handleFileSelect"
                accept=".jpg,.jpeg,.png,.pdf"
            />
            
            <p v-if="!file">
                Drag and drop file here, or click to select
            </p>
            
            <p v-else>
                {{ file.name }} ({{ formatSize(file.size) }})
            </p>
        </div>
        
        <!-- Upload Progress -->
        <div v-if="isUploading" class="progress-container">
            <div class="progress-bar">
                <div 
                    class="progress-fill"
                    :style="{ width: progress + '%' }"
                ></div>
            </div>
            <p>{{ progress }}% uploaded</p>
        </div>
        
        <!-- Upload Button -->
        <button 
            @click="uploadFile"
            :disabled="!file || isUploading"
            class="btn-upload"
        >
            {{ isUploading ? 'Uploading...' : 'Upload File' }}
        </button>
        
        <!-- Success Message -->
        <div v-if="uploadedUrl" class="success">
            <p>✓ File uploaded successfully</p>
            <a :href="uploadedUrl" target="_blank">View File</a>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';
import apiClient from '@/api/client';

export default {
    setup() {
        const fileInput = ref(null);
        const file = ref(null);
        const isDragging = ref(false);
        const isUploading = ref(false);
        const progress = ref(0);
        const uploadedUrl = ref(null);
        
        const handleDrop = (e) => {
            isDragging.value = false;
            const files = e.dataTransfer.files;
            if (files.length > 0) {
                file.value = files[0];
            }
        };
        
        const handleFileSelect = (e) => {
            file.value = e.target.files?.[0] || null;
        };
        
        const uploadFile = async () => {
            if (!file.value) return;
            
            const formData = new FormData();
            formData.append('file', file.value);
            
            isUploading.value = true;
            progress.value = 0;
            
            try {
                const response = await apiClient.post('/upload', formData, {
                    headers: { 'Content-Type': 'multipart/form-data' },
                    onUploadProgress: (progressEvent) => {
                        progress.value = Math.round(
                            (progressEvent.loaded / progressEvent.total) * 100
                        );
                    }
                });
                
                uploadedUrl.value = response.data.url;
                file.value = null;
                progress.value = 0;
                
                // Emit event to parent
                this.$emit('file-uploaded', response.data);
            } catch (err) {
                console.error('Upload failed:', err.message);
            } finally {
                isUploading.value = false;
            }
        };
        
        const formatSize = (bytes) => {
            if (bytes === 0) return '0 B';
            const k = 1024;
            const sizes = ['B', 'KB', 'MB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i];
        };
        
        return {
            fileInput,
            file,
            isDragging,
            isUploading,
            progress,
            uploadedUrl,
            handleDrop,
            handleFileSelect,
            uploadFile,
            formatSize
        };
    }
}
</script>

<style scoped>
.drop-zone {
    border: 2px dashed #ddd;
    padding: 40px;
    text-align: center;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s;
}

.drop-zone.is-dragging {
    border-color: #007bff;
    background: #f0f7ff;
}

.drop-zone input { display: none; }

.progress-container { margin-top: 20px; }

.progress-bar {
    width: 100%;
    height: 8px;
    background: #eee;
    border-radius: 4px;
    overflow: hidden;
}

.progress-fill {
    height: 100%;
    background: #007bff;
    transition: width 0.3s;
}

.btn-upload {
    margin-top: 20px;
    padding: 10px 20px;
    background: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.btn-upload:disabled { opacity: 0.5; cursor: not-allowed; }

.success {
    margin-top: 20px;
    padding: 15px;
    background: #d4edda;
    border-radius: 4px;
    color: #155724;
}
</style>
```

### 5.3 AWS S3 Integration

```javascript
// .env
AWS_ACCESS_KEY_ID=AKIAJ...
AWS_SECRET_ACCESS_KEY=abc...
AWS_S3_BUCKET=my-bucket-name
AWS_S3_REGION=ap-southeast-1

// src/services/s3Service.js
const AWS = require('aws-sdk');

const s3 = new AWS.S3({
    region: process.env.AWS_S3_REGION
});

module.exports = {
    uploadFile: async (buffer, fileName, mimeType) => {
        const params = {
            Bucket: process.env.AWS_S3_BUCKET,
            Key: `uploads/${Date.now()}-${fileName}`,
            Body: buffer,
            ContentType: mimeType
        };
        
        return s3.upload(params).promise();
    },
    
    deleteFile: async (key) => {
        return s3.deleteObject({
            Bucket: process.env.AWS_S3_BUCKET,
            Key: key
        }).promise();
    },
    
    getSignedUrl: (key, expiresIn = 3600) => {
        return s3.getSignedUrl('getObject', {
            Bucket: process.env.AWS_S3_BUCKET,
            Key: key,
            Expires: expiresIn
        });
    }
};
```

### 5.4 Chunked Upload for Large Files

```javascript
// Frontend: split file into chunks
async function uploadLargeFile(file) {
    const chunkSize = 5 * 1024 * 1024; // 5MB chunks
    const chunks = Math.ceil(file.size / chunkSize);
    const uploadId = generateUUID();
    
    for (let i = 0; i < chunks; i++) {
        const start = i * chunkSize;
        const end = Math.min(start + chunkSize, file.size);
        const chunk = file.slice(start, end);
        
        const formData = new FormData();
        formData.append('chunk', chunk);
        formData.append('chunkIndex', i);
        formData.append('totalChunks', chunks);
        formData.append('uploadId', uploadId);
        
        await apiClient.post('/upload-chunk', formData);
    }
}

// Backend: reassemble chunks
router.post('/upload-chunk', async (req, res) => {
    const { chunkIndex, totalChunks, uploadId } = req.body;
    
    // Save chunk temporarily
    await fs.writeFile(
        `/tmp/${uploadId}-${chunkIndex}`,
        req.file.buffer
    );
    
    if (chunkIndex === totalChunks - 1) {
        // All chunks received, reassemble
        const buffer = Buffer.concat(
            Array.from({ length: totalChunks }, (_, i) =>
                fs.readFileSync(`/tmp/${uploadId}-${i}`)
            )
        );
        
        // Upload to S3
        await s3Service.uploadFile(buffer, ...);
    }
    
    res.json({ success: true });
});
```

## 6. Studi Kasus Nyata: Gojek Driver KYC Document Upload

```javascript
// Gojek driver upload:
// - KTP (ID card) JPEG
// - License PDF
// - SIM (driving license) JPEG
// - Selfie with KTP

// Each document:
// - Validated (format, size)
// - Scanned (malware)
// - Stored securely
// - Expires after 1 year
```

## 7. Visualisasi: Upload Architecture

```
Browser
   ↓
[FormData: file]
   ↓
Express.js + Multer
   ├─ Validate file
   ├─ Virus scan
   └─ Store buffer
   ↓
AWS S3
   ├─ Encrypt
   └─ Store
   ↓
CloudFront CDN
   ├─ Cache
   └─ Deliver fast
   ↓
Client (via signed URL)
```

## 8. Kesalahan Umum

### ❌ Storing files locally

```javascript
// ❌ WRONG - not scalable
multer({ destination: './uploads/' });

// ✅ CORRECT - cloud storage
multer({ storage: memoryStorage() }) → S3
```

### ❌ No file validation

```javascript
// ❌ WRONG
app.post('/upload', upload.single('file'), (req, res) => {
    // accept any file
});

// ✅ CORRECT
fileFilter: (req, file, cb) => {
    if (!allowedTypes.includes(file.mimetype)) {
        return cb(new Error('Invalid type'));
    }
};
```

## 9. Latihan & Studi Kasus

### Latihan 1: File Upload Endpoint
```javascript
// Implement POST /upload with multer
// - Accept file
// - Validate (type, size)
// - Save to local disk
```

### Latihan 2: Progress Tracking
```javascript
// Frontend component with:
// - File input
// - Progress bar
// - Upload button
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami multipart/form-data
- [ ] Bisa setup multer
- [ ] Bisa validate files (type, size)
- [ ] Bisa integrate AWS S3
- [ ] Bisa implement upload progress
- [ ] Bisa handle errors
- [ ] Bisa generate signed URLs
- [ ] Bisa implement chunked uploads
- [ ] Mengerti file security
- [ ] Bisa scale to millions of uploads

## 11. Referensi

- [Multer Documentation](https://github.com/expressjs/multer)
- [AWS S3 SDK](https://docs.aws.amazon.com/sdk-for-javascript/)
- [OWASP File Upload Security](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)

**Status:** ✅ Pertemuan 12 selesai
