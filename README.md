# Carbon Blueprint AI 🌳

**Analyze construction blueprints for their carbon footprint and get AI-powered sustainability recommendations.**

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Flask](https://img.shields.io/badge/Flask-2.0+-green.svg)
![React](https://img.shields.io/badge/React-18.0+-61DAFB.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## 📋 Table of Contents

- Overview
- Features
- Architecture
- Project Structure
- Installation
- Usage
- How It Works
- API Documentation
- Technologies Used
- Contributing
- License

---

## 🌍 Overview

**Carbon Blueprint AI** is a full-stack web application designed to help architects, engineers, and construction professionals assess the environmental impact of their building projects. By uploading construction blueprints (PDF format), users receive:

- **Carbon Footprint Analysis**: Detailed CO₂ emissions breakdown by material and room
- **Material Insights**: Understanding which materials contribute most to emissions
- **Sustainability Recommendations**: AI-powered suggestions for reducing carbon impact through material substitution, design optimization, and construction practices

This tool empowers the construction industry to make data-driven decisions toward more sustainable building practices.

---

## ✨ Features

### 🔍 **Intelligent Blueprint Parsing**
- Extracts room dimensions, names, and materials from PDF blueprints
- Hybrid parsing using dimension analysis and keyword-based fallback
- Supports complex floor plans with multiple rooms

### 📊 **Carbon Footprint Calculation**
- Calculates CO₂ emissions based on material types and quantities
- Uses a comprehensive material emission database (`material_db.json`)
- Provides per-room and total emission breakdowns

### 💡 **Smart Recommendations**
- **Material Substitutions**: Suggests low-carbon alternatives (e.g., mass timber, recycled steel, low-carbon concrete)
- **Design Enhancements**: Room-specific recommendations (daylighting, natural ventilation, green walls)
- **Construction Best Practices**: Waste management, local sourcing, and sustainable practices

### 🎨 **Modern User Interface**
- Drag-and-drop file upload with visual feedback
- Real-time analysis with loading indicators
- Clean, professional report cards with emissions visualization
- Responsive design for desktop and mobile devices

---

## 🏗️ Architecture

```
┌─────────────────┐
│  React Frontend │
│   (Port 3000)   │
└────────┬────────┘
         │ HTTP POST /api/analyze
         ▼
┌─────────────────┐
│  Flask Backend  │
│   (Port 5000)   │
└────────┬────────┘
         │
         ├─► Blueprint Parser (PDF → Room Data)
         │
         ├─► Carbon Calculator (Room Data → Emissions)
         │
         └─► Recommender (Emissions → Suggestions)
```

### **Data Flow**

1. **User uploads PDF** → Frontend (`FileUpload.js`)
2. **File sent to backend** → API endpoint (`/api/analyze`)
3. **Blueprint parsing** → `blueprint_parser.py` extracts room data
4. **Carbon calculation** → `carbon_calculator.py` computes emissions
5. **Generate recommendations** → `recommender.py` suggests improvements
6. **Return JSON report** → Frontend displays results (`Report.js`)

---

## 📁 Project Structure

```
gt_project/
│
├── backend/                      # Flask backend server
│   ├── app.py                    # Main Flask application
│   ├── modules/                  # Core processing modules
│   │   ├── blueprint_parser.py   # PDF parsing & room extraction
│   │   ├── carbon_calculator.py  # CO₂ emissions calculation
│   │   ├── recommender.py        # Sustainability recommendations
│   │   └── utils.py              # Utility functions
│   ├── data/
│   │   └── uploads/              # Uploaded blueprint storage
│   └── requirements.txt          # Python dependencies
│
├── frontend/                     # React frontend application
│   ├── src/
│   │   ├── components/
│   │   │   ├── FileUpload.js     # File upload component
│   │   │   └── Report.js         # Results display component
│   │   ├── api.js                # API communication
│   │   ├── App.js                # Main React component
│   │   ├── App.css               # Application styles
│   │   └── index.js              # React entry point
│   ├── public/                   # Static assets
│   ├── build/                    # Production build
│   └── package.json              # Node dependencies
│
├── data/
│   └── material_db.json          # Material emission factors database
│
├── outputs/
│   └── report.json               # Sample output reports
│
├── requirements.txt              # Root Python dependencies
└── README.md                     # Project documentation
```

---

## 🚀 Installation

### **Prerequisites**

- **Python 3.8+** ([Download](https://www.python.org/downloads/))
- **Node.js 14+** and **npm** ([Download](https://nodejs.org/))
- **Git** ([Download](https://git-scm.com/))

### **Step 1: Clone the Repository**

```bash
git clone https://github.com/yourusername/gt_project.git
cd gt_project
```

### **Step 2: Backend Setup**

```bash
# Navigate to backend directory
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### **Step 3: Frontend Setup**

```bash
# Navigate to frontend directory
cd ../frontend

# Install dependencies
npm install
```

---

## 💻 Usage

### **Development Mode**

#### **1. Start the Backend Server**

```bash
cd backend
# Activate virtual environment if not already active
python -m flask run --port=5000
```

The Flask server will run at `http://localhost:5000`

#### **2. Start the Frontend Development Server**

```bash
cd frontend
npm start
```

The React app will open at `http://localhost:3000`

### **Production Mode**

#### **1. Build the Frontend**

```bash
cd frontend
npm run build
```

#### **2. Start the Flask Server**

```bash
cd backend
python app.py
```

Access the full application at `http://localhost:5000`

### **Uploading Blueprints**

1. **Open the application** in your browser
2. **Drag and drop** a PDF blueprint or click to browse
3. **Click "Analyze"** to process the blueprint
4. **View results**: 
   - Total CO₂ emissions
   - Material breakdown by room
   - Personalized sustainability recommendations

---

## 🔬 How It Works

### **1. Blueprint Parsing** (`blueprint_parser.py`)

Uses **pdfplumber** to extract:
- **Room numbers** (e.g., "101", "102")
- **Dimensions** in feet and inches (e.g., "12'6\" × 10'3\"")
- **Spatial relationships** between room labels and dimensions

**Algorithm:**
1. Extract all text with coordinates from PDF
2. Identify room numbers using regex (`^\d{3}$`)
3. Find dimension strings with feet/inches notation
4. Match dimensions to nearest room based on proximity
5. Calculate room area (length × width)
6. Apply fallback keyword matching for rooms without dimensions

### **2. Carbon Calculation** (`carbon_calculator.py`)

**Formula:**
```
CO₂ Emissions = Area (sq ft) × Emission Factor (kg CO₂e/sq ft)
```

**Emission Factors** (from `material_db.json`):
- **Concrete**: 0.74 kg CO₂e/sq ft
- **Steel**: 2.85 kg CO₂e/sq ft
- **Wood**: 0.32 kg CO₂e/sq ft
- **Glass**: 1.12 kg CO₂e/sq ft
- **General Construction**: 0.50 kg CO₂e/sq ft (default)

### **3. Recommendation Engine** (`recommender.py`)

Generates three types of recommendations:

**Material Substitutions:**
```python
"concrete" → "Consider Mass Timber (CLT) or low-carbon concrete with fly ash"
"steel" → "Specify Electric Arc Furnace (EAF) recycled steel"
```

**Design Enhancements** (room-specific):
```python
"office" → "Maximize natural daylight with proper window orientation"
"conference room" → "Install operable windows for natural ventilation"
```

**General Practices:**
- Waste management and recycling
- Local material sourcing
- Deconstruction planning

---

## 📡 API Documentation

### **POST `/api/analyze`**

Analyzes an uploaded blueprint and returns carbon footprint data.

**Request:**
```http
POST /api/analyze
Content-Type: multipart/form-data

file: [PDF blueprint file]
```

**Response:**
```json
{
  "blueprint_data": {
    "rooms": [
      {
        "name": "Office 101",
        "area": 120,
        "material": "general construction"
      }
    ]
  },
  "carbon_analysis": {
    "materials": [
      {
        "name": "Office 101",
        "material": "general construction",
        "quantity": 120,
        "unit": "sqft",
        "emission": 60.0
      }
    ],
    "total_emissions": 60.0
  },
  "recommendations": [
    "--- Material Suggestions ---",
    "**Material**: Consider low-carbon alternatives...",
    "--- Blueprint & Design Enhancements ---",
    "**Design**: Maximize natural daylight...",
    "--- General Construction Practices ---",
    "**Construction**: Implement waste management..."
  ]
}
```

**Error Response:**
```json
{
  "error": "Error message description"
}
```

---

## 🛠️ Technologies Used

### **Backend**
- **Flask** - Lightweight web framework
- **pdfplumber** - PDF text and layout extraction
- **Flask-CORS** - Cross-origin resource sharing
- **Werkzeug** - Secure file handling

### **Frontend**
- **React 18** - UI library
- **Axios** - HTTP client
- **CSS3** - Custom styling with CSS variables

### **Data Processing**
- **Python regex** - Pattern matching for dimensions
- **JSON** - Material database storage

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

### **Areas for Improvement**
- [ ] Support for DWG and IFC file formats
- [ ] Machine learning for better room detection
- [ ] Integration with BIM software
- [ ] Multi-language support
- [ ] PDF report generation
- [ ] User authentication and project management

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🙏 Acknowledgments

- Material emission factors based on industry standards
- Inspired by sustainable construction practices
- Built with ❤️ for a greener future

---

## 📧 Contact

**Project Maintainer**: Aditya Raj   
**GitHub**: [Aditya Raj](https://github.com/aditya-46-raj)

---

<p align="center">
  <strong>🌱 Building a sustainable future, one blueprint at a time.</strong>
</p>
