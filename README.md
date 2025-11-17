# Predictive-Transaction-Intelligence-system-for-BFSI
Model Components
📦 Prerequisites
🚀 Installation
1. Clone the Repository
2. Backend Setup
Create Virtual Environment
Activate Virtual Environment
Windows (PowerShell):
Technology: HTML5, JavaScript (ES6+), Tailwind CSS
Architecture: Single-Page Application (SPA)
API Communication: Fetch API
No Build Process: Direct browser execution
1. Fraud Detection Model: fraud_detector.pkl (scikit-learn model)
2. Feature Scaler: scaler.pkl (StandardScaler)
3. Chatbot Model:
Base: microsoft/Phi-3-mini-4k-instruct
Fine-tuned: LoRA adapters from phi3-bfsi-finetuned
Python: 3.8 or higher
Node.js: Not required (frontend runs directly in browser)
Operating System: Windows, Linux, or macOS
Memory: Minimum 8GB RAM (16GB+ recommended for chatbot)
Storage: ~5GB for models and dependencies
git clone <repository-url>
cd BFSI-Predictive-Intelligence
cd bfsi_backend
python -m venv venv
.\venv\Scripts\Activate.ps1
Windows (CMD):
Linux/macOS:
Install Dependencies
3. Model Files
Ensure the following model files are present in bfsi_backend/app/models/ :
4. Chatbot Model Configuration
Update the LoRA adapter path in bfsi_backend/app/chatbot_wrapper.py :
5. CSV Data Path (Optional)
If using the CSV viewer, update the CSV file path in bfsi_backend/app/main.py :
⚙ Configuration
Backend Configuration
Port Configuration
Edit bfsi_backend/app.py to change the server port:
venv\Scripts\activate.bat
source venv/bin/activate
pip install -r requirements.txt
fraud_detector.pkl - Trained fraud detection model
scaler.pkl - Feature scaler for preprocessing
LORA_ADAPTER_PATH = r"<path-to-your-phi3-bfsi-finetuned-adapters>"
csv_path = r"<path-to-your-csv-file>"
uvicorn.run(app, host="0.0.0.0", port=8000) # Change port here
CORS Configuration
CORS is configured to allow all origins. For production, update bfsi_backend/app/main.py :
Frontend Configuration
API Base URL
Update Front-End/script.js and Front-End/main.js if backend runs on different host/port:
💻 Usage
Starting the Backend Server
The server will start on http://0.0.0.0:8000 (accessible at http://127.0.0.1:8000 )
Accessing the Frontend
app.add_middleware(
CORSMiddleware,
allow_origins=["*"], # Change to specific domains in production
allow_credentials=True,
allow_methods=["*"],
allow_headers=["*"],
)
const API_BASE_URL = 'http://127.0.0.1:8000'; // Change if needed
1. Navigate to the backend directory:
cd bfsi_backend
2. Activate virtual environment (if not already activated)
3. Start the server:
python app.py
1. Open Front-End/index.html in a web browser
Option 1: Double-click the file
Option 2: Use a local web server:
Using Fraud Detection
Using the Chatbot
Viewing CSV Data
# Python 3
cd Front-End
python -m http.server 8080
# Then open http://localhost:8080
2. Navigate through the application:
Home: Overview of platform features
Fraud Detection: Submit transaction data for fraud analysis
Chatbot: Interact with the BFSI chatbot
Data Viewer: View transaction data from CSV
Contact/About: Information about the platform
1. Navigate to "Fraud Detection" page
2. Fill in the transaction form with:
User ID, Amount, Category
Merchant ID, Device ID
Transaction date and time
Location, Currency, Card Type
Transaction status and authentication method
Velocity features (previous transactions, distance, time)
3. Click "Detect Fraud"
4. View results showing:
Fraud prediction (Fraud/Normal)
Probability score
1. Navigate to "Chatbot" page
2. Type your question in the input field
3. Press Enter or click the send button
4. Wait for the AI response
5. Continue the conversation
1. Navigate to "Data Viewer" page
2. Data automatically loads from the backend
3. View transaction data in tabular format
📡 API Documentation
Base URL
Endpoints
1. Health Check
Response:
2. Fraud Detection
Request Body:
Response:
Feature Vector Order:
4. First 100 rows are displayed
http://127.0.0.1:8000
GET /
{
"status": "ok",
"service": "BFSI Predictive Intelligence API"
}
POST /predict_fraud
{
"feature_vector": [1250.75, 10, 445, 876, 1, 0, 2, 12, 50.2, 15.5, 1, 3, 3,
2024, 1, 15, 14]
}
{
"is_fraud": 0,
"probability": 0.234
}
3. Chatbot
Request Body:
Response:
4. CSV Data
1. Transaction_Amount
2. Transaction_Location (encoded)
3. Merchant_ID
4. Device_ID
5. Card_Type (encoded)
6. Transaction_Currency (encoded)
7. Transaction_Status (encoded)
8. Previous_Transaction_Count
9. Distance_Between_Transactions_km
10. Time_Since_Last_Transaction_min
11. Authentication_Method (encoded)
12. Transaction_Velocity
13. Transaction_Category (encoded)
14. Year
15. Month
16. Day
17. Hour
POST /chat
{
"message": "What is a credit score?",
"history": [] // Optional
}
{
"reply": "A credit score is a numerical representation...",
"meta": {
"source": "phi3-bfsi-lora"
}
}
Response:
📁 Project Structure
🔧 Troubleshooting
Backend Issues
Port Already in Use
Error: [Errno 10048] error while attempting to bind on address
GET /csv_data
{
"data": [...],
"columns": ["col1", "col2", ...],
"total_rows": 1000,
"displayed_rows": 100
}
BFSI-Predictive-Intelligence/

 bfsi_backend/ # Backend application
  app/
   __init__.py
   main.py # FastAPI application and routes
   chatbot_wrapper.py # Chatbot model wrapper
   models/ # ML model files
   fraud_detector.pkl
   scaler.pkl
  app.py # Server entry point
  requirements.txt # Python dependencies
  venv/ # Virtual environment (not in git)

 Front-End/ # Frontend application
  index.html # Main HTML file
  script.js # Main JavaScript logic
  main.js # Fraud detection module
  encoders_full.js # Feature encoders
  style.css # Custom styles

 README.md # This file
Solution (Windows):
Solution (Linux/macOS):
Model Files Not Found
Error: Could not load scaler/fraud model
Solution: Ensure fraud_detector.pkl and scaler.pkl are in bfsi_backend/app/models/
Chatbot Not Initializing
Error: Chatbot not initialized on server
Solutions:
Import Errors
Error: Module not found
Solution: Ensure virtual environment is activated and dependencies are installed:
Frontend Issues
Cannot Connect to Backend
# Find process using port 8000
netstat -ano | findstr :8000
# Kill the process (replace PID with actual process ID)
taskkill /PID <PID> /F
# Find process using port 8000
lsof -ti:8000
# Kill the process
kill -9 $(lsof -ti:8000)
1. Check LoRA adapter path in chatbot_wrapper.py
2. Verify model files are accessible
3. Check available memory (chatbot requires significant RAM)
4. Review backend logs for specific error messages
pip install -r requirements.txt
Error: Failed to fetch or connection errors
Solutions:
Feature Encoding Errors
Error: Undefined encoder values
Solution: Verify all categorical features have corresponding encoders in script.js or
encoders_full.js
CSV Data Not Loading
Error: CSV file not found
Solution: Update CSV path in bfsi_backend/app/main.py to point to your CSV file
Performance Issues
Slow Chatbot Responses
High Memory Usage
🤝 Contributing
1. Verify backend server is running on port 8000
2. Check API_BASE_URL in script.js matches backend URL
3. Check browser console for CORS errors
4. Ensure firewall allows connections
Chatbot runs on CPU, which is slower than GPU
Consider reducing max_new_tokens in chatbot_wrapper.py
Close other memory-intensive applications
Chatbot model requires significant RAM
Consider using a smaller model or GPU acceleration
Monitor system resources
1. Fork the repository
2. Create a feature branch ( git checkout -b feature/AmazingFeature )
3. Commit your changes ( git commit -m 'Add some AmazingFeature' )
4. Push to the branch ( git push origin feature/AmazingFeature )
5. Open a Pull Request
📝 License
[Specify your license here]
👥 Authors
[Your name/team name]
🙏 Acknowledgments
📞 Support
For issues, questions, or contributions, please:
Note: This is a development/demonstration platform. For production use, ensure proper
security measures, error handling, and model validation are implemented
