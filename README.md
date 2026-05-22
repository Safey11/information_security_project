# SafeyKit 🔐

**A Professional Security Toolkit for Developers and Security Enthusiasts**

SafeyKit is a comprehensive, open-source security toolkit built with Flask. It provides essential cryptographic utilities, password analysis, encoding/decoding tools, and breach detection capabilities in a sleek, modern interface.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Python 3.7+](https://img.shields.io/badge/Python-3.7%2B-blue)
![Flask](https://img.shields.io/badge/Framework-Flask-green)

---

**live Project:** https://infosecpj.vercel.app/

## 🚀 Features

### 🔑 Password Tools
- **Secure Password Generator**: Generate strong, cryptographically secure passwords with customizable options (uppercase, lowercase, numbers, symbols)
- **Ambiguous Character Exclusion**: Option to exclude similar-looking characters (O/0, I/1, l/1, etc.)
- **Strength Analyzer**: Real-time password strength analysis with:
  - Entropy calculation
  - Crack time estimation
  - Detailed criteria breakdown
  - Color-coded strength indicators

### 🛡️ Security Features
- **Breach Checker**: Verify if passwords have been compromised using the Have I Been Pwned API (privacy-safe implementation)
- **Hash Generation**: Support for 9+ hashing algorithms:
  - MD5, SHA-1, SHA-256, SHA-384, SHA-512
  - SHA3-256, SHA3-512
  - BLAKE2b, BLAKE2s
- **HMAC Generator**: Create HMAC signatures with multiple algorithms

### 🔐 Cipher Suite
- **Caesar Cipher**: Classic shift cipher with brute-force cracking
- **Vigenere Cipher**: Polyalphabetic substitution cipher
- **Atbash Cipher**: Simple substitution cipher
- **ROT13**: Variant of Caesar cipher with fixed shift
- **Morse Code**: Encode/decode messages to Morse code

### 🔄 Encoding & Decoding
- **Base64**: Standard base64 encoding/decoding
- **Hex**: Hexadecimal encoding/decoding
- **Binary**: Convert text to/from binary representation
- **URL Encoding**: URL-safe character encoding

### 📊 Conversion Tools
- **Number Base Converter**: Convert between binary, octal, decimal, hexadecimal, and up to base-36
- **Multi-format Display**: View results in multiple base formats simultaneously

---

## 📋 Requirements

- Python 3.7 or higher
- Flask
- Requests library

---

## 🛠️ Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/safeykit.git
cd safeykit
```

### 2. Create Virtual Environment (Recommended)
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the Application
```bash
python app.py
```

The application will start on `http://localhost:5000`

---

## 📖 Usage Guide

### Web Interface

Visit `http://localhost:5000` to access the SafeyKit interface. The UI features:
- **Dark theme** with modern, cyberpunk-inspired design
- **Intuitive navigation** between different security tools
- **Real-time feedback** for password analysis and conversions
- **Responsive design** for desktop and mobile devices

### API Endpoints

SafeyKit provides REST API endpoints for programmatic access:

#### Password Generation
```bash
POST /api/password/generate
Content-Type: application/json

{
  "length": 16,
  "upper": true,
  "lower": true,
  "nums": true,
  "syms": true,
  "exclude_ambiguous": false
}
```

Response:
```json
{
  "password": "kR7@mP2xL9$qW5nJ",
  "strength": {
    "score": 8,
    "label": "Excellent",
    "color": "#00f0c0",
    "pct": 100,
    "entropy": 95.2,
    "crack_time": "1.2B+ years"
  }
}
```

#### Password Strength Analysis
```bash
POST /api/password/strength
Content-Type: application/json

{
  "password": "MySecurePass123!"
}
```

#### Hash Generation
```bash
POST /api/hash/generate
Content-Type: application/json

{
  "text": "Hello World",
  "algorithm": "sha256"
}
```

Supported algorithms: `md5`, `sha1`, `sha224`, `sha256`, `sha384`, `sha512`, `sha3_256`, `sha3_512`, `blake2b`, `blake2s`, `all`

#### Breach Checker
```bash
POST /api/breach
Content-Type: application/json

{
  "password": "test@password"
}
```

Response (if compromised):
```json
{
  "pwned": true,
  "count": 123456,
  "count_formatted": "123,456",
  "severity": "critical"
}
```

#### Encoding/Decoding
```bash
POST /api/encode
Content-Type: application/json

{
  "text": "Hello World",
  "method": "base64",
  "action": "encode"
}
```

Supported methods: `base64`, `hex`, `binary`, `url`

#### Cipher Operations
```bash
POST /api/cipher
Content-Type: application/json

{
  "text": "Hello World",
  "cipher": "caesar",
  "shift": 3,
  "action": "encode"
}
```

Supported ciphers: `caesar`, `caesar_brute`, `vigenere`, `atbash`, `rot13`, `morse`, `hmac`

#### Number Base Conversion
```bash
POST /api/convert
Content-Type: application/json

{
  "value": "255",
  "from_base": 10,
  "to_base": 16
}
```

Supported bases: 2-36

---

## 📂 Project Structure

```
safeykit/
├── app.py                 # Flask application & core logic
├── requirements.txt       # Python dependencies
├── templates/
│   └── index.html        # Web interface
└── README.md             # This file
```

---

## 🔒 Security Considerations

### Best Practices
- ✅ Uses `secrets` module for cryptographically secure random generation
- ✅ Implements safe Base64 and encoding operations
- ✅ Breach checking uses privacy-safe k-anonymity (partial hash matching)
- ✅ All operations are performed locally (except breach API)
- ✅ No password data is stored or logged

### Privacy
- Passwords are NOT stored or transmitted unnecessarily
- Breach API only receives the first 5 characters of the SHA-1 hash
- No telemetry or tracking is implemented
- All sensitive operations happen client-side or securely

### Limitations
- Single-threaded by default (suitable for local/small deployments)
- Not a replacement for dedicated password managers
- For production use, deploy behind a reverse proxy with HTTPS

---

## 🚀 Deployment

### Local Development
```bash
python app.py
```

### Production with Gunicorn (Recommended)
```bash
pip install gunicorn
gunicorn --workers 4 --bind 0.0.0.0:5000 app:app
```

### Docker
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

Build and run:
```bash
docker build -t safeykit .
docker run -p 5000:5000 safeykit
```

---

## 📚 Examples

### Generate a Strong Password
```python
from app import generate_password, analyze_strength

password, error = generate_password(length=24, exclude_ambiguous=True)
strength = analyze_strength(password)
print(f"Password: {password}")
print(f"Strength: {strength['label']}")
```

### Hash Multiple Algorithms
```python
from app import generate_all_hashes

text = "Important Data"
hashes = generate_all_hashes(text)
for algo, hash_value in hashes.items():
    print(f"{algo}: {hash_value}")
```

### Cipher Text
```python
from app import caesar_cipher, vigenere_cipher

text = "Secret Message"
encrypted = caesar_cipher(text, shift=5)
print(f"Caesar: {encrypted}")

encrypted_vig = vigenere_cipher(text, key="KEY")
print(f"Vigenere: {encrypted_vig}")
```

---

## 🤝 Contributing

Contributions are welcome! Here's how to help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Guidelines
- Follow PEP 8 style guide
- Add docstrings to functions
- Test new features thoroughly
- Update README for new features

---

## 📝 License

This project is licensed under the **MIT License** - see the LICENSE file for details.

---

## 🐛 Bug Reports & Feature Requests

Found a bug or have a feature suggestion?
- **Open an Issue** on GitHub with detailed description
- **Include** steps to reproduce (for bugs)
- **Tag** appropriately (bug, enhancement, documentation)

---

## 📞 Support

- 📧 **Email**: [Your Email]
- 💬 **GitHub Issues**: [Project Issues Page]
- 🌐 **Wiki**: [Project Wiki Page]

---

## 🎯 Roadmap

- [ ] WebAssembly version for offline usage
- [ ] Command-line interface (CLI)
- [ ] Password history management
- [ ] Batch processing tools
- [ ] Dark/Light theme toggle
- [ ] Multi-language support
- [ ] RSA encryption/decryption
- [ ] Digital signature generation

---

## ⭐ Acknowledgments

- **Have I Been Pwned API** - For breach detection service
- **Font sources**: Google Fonts (Rajdhani, JetBrains Mono, Exo 2)
- **Community** - For feedback and contributions

---

## 📊 Statistics

- **Tools Available**: 15+
- **Algorithms Supported**: 20+
- **API Endpoints**: 8
- **Lines of Code**: 500+

---

## 🔐 Disclaimer

This tool is provided for **educational and authorized security purposes only**. Users are responsible for:
- Ensuring they have permission to use this tool
- Complying with applicable laws and regulations
- Understanding the limitations and security implications

**The developers are NOT responsible for misuse or damage caused by this software.**

---

**Made with ❤️ for the security community**

Last Updated: 2026-05-22
