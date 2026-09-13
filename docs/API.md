# API Documentation 📚

## Base URL
```
http://localhost:5000
```

## التوثيق الكامل للـ API

### 1. Text to Video
#### إنشاء فيديو من نص

**Endpoint:** `POST /api/generate/text`

**Request Body:**
```json
{
  "prompt": "مقطع فيديو يظهر شروق الشمس على المحيط",
  "duration": 10,
  "model": "openai_dalle3",
  "style": "cinematic",
  "resolution": "1080p"
}
```

**Response:**
```json
{
  "status": "processing",
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "message": "Video generation started",
  "prompt": "مقطع فيديو يظهر شروق الشمس على المحيط",
  "duration": 10,
  "model": "openai_dalle3",
  "style": "cinematic",
  "resolution": "1080p",
  "created_at": "2024-01-15T10:30:00"
}
```

---

### 2. Image to Video
#### إنشاء فيديو من صورة

**Endpoint:** `POST /api/generate/image`

**Request:** multipart/form-data
```
- image: <file>
- prompt: Animate this image naturally
- duration: 5
- model: runway_gen2
```

**Response:**
```json
{
  "status": "processing",
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "message": "Video generation started from image",
  "prompt": "Animate this image naturally",
  "duration": 5,
  "model": "runway_gen2",
  "created_at": "2024-01-15T10:30:00"
}
```

---

### 3. Add Voiceover
#### إضافة صوت للفيديو

**Endpoint:** `POST /api/audio/add-voiceover`

**Request:** multipart/form-data
```
- video: <file>
- text: هذا نص للتحويل إلى كلام
- language: ar
- model: openai_tts
```

**Response:**
```json
{
  "status": "processing",
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "message": "Voiceover generation started",
  "text": "هذا نص للتحويل إلى كلام",
  "language": "ar",
  "model": "openai_tts",
  "created_at": "2024-01-15T10:30:00"
}
```

---

### 4. Add Music
#### إضافة موسيقى للفيديو

**Endpoint:** `POST /api/audio/add-music`

**Request:** multipart/form-data
```
- video: <file>
- audio: <file>
- volume: 70
- fade_in: 1.0
- fade_out: 2.0
```

**Response:**
```json
{
  "status": "processing",
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "message": "Music addition started",
  "volume": 70,
  "fade_in": 1.0,
  "fade_out": 2.0,
  "created_at": "2024-01-15T10:30:00"
}
```

---

### 5. Get Status
#### الحصول على حالة المعالجة

**Endpoint:** `GET /api/generate/text/status/<job_id>`

**Response:**
```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "processing",
  "progress": 45,
  "estimated_time": 30
}
```

---

### 6. Available Models
#### الحصول على نماذج متاحة

**Endpoint:** `GET /api/models`

**Response:**
```json
{
  "text_to_video": [
    {"name": "OpenAI DALL-E 3", "id": "openai_dalle3"},
    {"name": "Replicate Zeroscope", "id": "replicate_zeroscope"}
  ],
  "image_to_video": [
    {"name": "Runway Gen-2", "id": "runway_gen2"},
    {"name": "Replicate SVD", "id": "replicate_svd"}
  ],
  "audio": [
    {"name": "OpenAI TTS", "id": "openai_tts"},
    {"name": "ElevenLabs", "id": "elevenlabs"}
  ]
}
```

---

### 7. Health Check
#### فحص حالة الخدمة

**Endpoint:** `GET /health`

**Response:**
```json
{
  "status": "healthy",
  "service": "AI Video Generator",
  "version": "1.0.0"
}
```

---

## Error Handling

جميع الأخطاء يتم إرجاعها بصيغة JSON:

```json
{
  "error": "Error message",
  "message": "Detailed error message"
}
```

### Status Codes
- `200` - Success
- `202` - Accepted (Processing)
- `400` - Bad Request
- `404` - Not Found
- `500` - Internal Server Error

---

## Rate Limiting

- حد أقصى 100 طلب لكل ساعة لكل IP
- للحسابات المميزة: بدون حد

---

## Authentication

استخدم API Key في Header:
```
Authorization: Bearer YOUR_API_KEY
```

---

## Examples

### cURL
```bash
# Text to Video
curl -X POST http://localhost:5000/api/generate/text \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "فيديو جميل",
    "duration": 10
  }'
```

### Python
```python
import requests

response = requests.post(
    'http://localhost:5000/api/generate/text',
    json={
        'prompt': 'فيديو جميل',
        'duration': 10
    }
)

print(response.json())
```

### JavaScript
```javascript
const response = await fetch('/api/generate/text', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    prompt: 'فيديو جميل',
    duration: 10
  })
});

const data = await response.json();
console.log(data);
```
